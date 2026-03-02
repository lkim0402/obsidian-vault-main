- Taken from assignment

```yaml
name: CD Workflow - Build & Push to ECR  
  
on:  
  push:  
    branches: [ "release" ]  
  
jobs:  
  # ----------------------------------------------------------------  
  # JOB 1: Docker 이미지를 빌드하고 ECR에 푸시  
  # ----------------------------------------------------------------  
  build-and-push:  
    runs-on: ubuntu-latest  
  
	# give GITHUB_TOKEN to be able to read ONLY
    permissions:  
      contents: read  
  
    # `outputs` block is used to **pass data from one job to another job** within the same workflow
    outputs:  
      #      repo_uri: ${{ steps.prep.outputs.repo_uri }}  
      image_tag: ${{ steps.prep.outputs.image_tag }}  
  
    steps:  
      # 소스 체크아웃  
      - name: Checkout  
        uses: actions/checkout@v3  
  
      # AWS 자격 증명 설정 (secrets 사용)  
      - name: AWS CLI 설정  
        uses: aws-actions/configure-aws-credentials@v2  
        with:  
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}  
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}  
          aws-region: us-east-1  
  
      # ECR 로그인 (계정 ID를 Secret에서 참조)  
      - name: Login to Amazon ECR Public  
        id: login-ecr-public  
        uses: aws-actions/amazon-ecr-login@v2  
        with:  
          registry-type: public  
  
      # (선택) 이미지 태그 구성 - 커밋 해시 7자리 +latest      - name: Prepare tags  
        id: prep  
        run: |  
          echo "IMAGE_TAG=${GITHUB_SHA::7}" >> $GITHUB_ENV  
          echo "REPO_URI=${{ secrets.ECR_REPOSITORY_URI }}" >> $GITHUB_ENV  
  
          echo "IMAGE_TAG=${GITHUB_SHA::7}" >> $GITHUB_OUTPUT  
  
      # Docker 빌드 (dockerfile 사용)  
      - name: Docker build  
        run: |  
          docker build -t "$REPO_URI:$IMAGE_TAG" -t "$REPO_URI:latest" .  
  
      # ECR 푸쉬  
      - name: Docker push  
        run: |  
          docker push "$REPO_URI:$IMAGE_TAG"  
          docker push "$REPO_URI:latest"  
  # ----------------------------------------------------------------  
  # JOB 2: 새 이미지를 ECS 서비스에 배포  
  # ----------------------------------------------------------------  
  deploy-to-ecs: 
	  ... 
```

- `permissions: contents: read`
	- grants the job **read-only** permission to your repository's contents
	- The `actions/checkout@v3` step needs this permission to fetch your source code. By restricting it to `read`, you prevent this job from being able to, for example, push code back to your repository, which is a great security practice
	- *mostly ONLY used for the checkout stage*!
- `outputs:  image_tag: ${{ steps.prep.outputs.image_tag }}  `
	- `outputs` block is used to **pass data from one job to another job** within the same workflow
	- pipeline where one job builds a Docker image, and a _second_ job deploys that specific image to a server (like Amazon ECS or Kubernetes). The second job needs to know the exact tag of the image that the first job just built
	- the line `echo "IMAGE_TAG=${GITHUB_SHA::7}" >> $GITHUB_OUTPUT` creates an output variable named `IMAGE_TAG` for that step
		- for job outputs, it's *case-insensitive*, you could also do this:   `image_tag: ${{steps.prep.outputs.IMAGE_TAG }}`
		- for `run:` steps, its *case-SENSITIVE* 
			- `echo "IMAGE_TAG=${GITHUB_SHA::7}" >> $GITHUB_ENV # Sets the ENV var`
- `name: Checkout, uses: actions/checkout@v3`
	- official action that **downloads (or "checks out") a copy of your repository's source code** onto the virtual machine (called a "runner")
		- the runner = a brand new empty computer that Github spins up just for your job (no code by default), Github provides for you when your workflow starts
	- "Go get all the project files so the next steps can build, test, or deploy them."
	- It's almost always the first step in any workflow that needs to work with your code
- `run: |`
	- The pipe `|` after `run:` is YAML syntax for a **multi-line string**
	- tells the YAML parser that the following indented lines should be treated as a single block of text, which is then executed as a shell script
	- `echo "IMAGE_TAG=${GITHUB_SHA::7}" >> $GITHUB_ENV`
		- `GITHUB_SHA`: This is a default variable provided by GitHub Actions. It contains the full 40-character commit hash that triggered the workflow
		- `:7`: This is shell syntax for creating a **substring**. It means "give me the first 7 characters" of the `GITHUB_SHA` variable
- `GITHUB_ENV` VS `GITHUB_OUTPUT`
	- The script writes the same `IMAGE_TAG` information to two different special files, each with a different purpose
	- `echo "..." >> $GITHUB_ENV` 
		- sets **environment variables for subsequent steps within the _same job_**
	- `echo "..." >> $GITHUB_OUTPUT` 
		- sets **outputs for the _entire job_**
		- This is how you pass values from this job to a _different job_ later in the workflow
# Job 2
```yaml
  ...
  # ----------------------------------------------------------------  
  # JOB 2: 새 이미지를 ECS 서비스에 배포  
  # ----------------------------------------------------------------  
  deploy-to-ecs:  
    runs-on: ubuntu-latest  
    needs: build-and-push  
  
    steps:  
      # AWS 자격 증명 설정 (여기서도 필요)  
      - name: AWS CLI 설정  
        uses: aws-actions/configure-aws-credentials@v2  
        with:  
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}  
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}  
          aws-region: ${{ secrets.AWS_REGION }}  
  
      # 기존 ECS 태스크 정의 다운로드  
      - name: Download current task definition  
        run: |  
          aws ecs describe-task-definition --task-definition ${{ secrets.ECS_TASK_DEFINITION }} \  
            --query taskDefinition > task-def.json  
  
      # 새 ECR 이미지로 태스크 정의 업데이트  
      - name: Fill in new image ID in task definition  
        id: render-task-def  
        uses: aws-actions/amazon-ecs-render-task-definition@v1  
        with:  
          task-definition: task-def.json  
          container-name: ${{ secrets.ECS_CONTAINER_NAME }}  
          image: ${{ secrets.ECR_REPOSITORY_URI }}:${{ needs.build-and-push.outputs.image_tag }}  
  
      # 새 태스크 정의 등록 및 ECS 서비스 업데이트  
      - name: Deploy Amazon ECS task definition  
        uses: aws-actions/amazon-ecs-deploy-task-definition@v2  
        with:  
          task-definition: ${{ steps.render-task-def.outputs.task-definition }}  
          service: ${{ secrets.ECS_SERVICE }}  
          cluster: ${{ secrets.ECS_CLUSTER }}  
          wait-for-service-stability: false  
  
      # 배포 상태 확인  
      - name: ECS 배포 상태 확인  
        run: |  
          aws ecs describe-services \  
            --cluster ${{ secrets.ECS_CLUSTER }} \  
            --services ${{ secrets.ECS_SERVICE }} \  
            --query "services[0].deployments"
```

- `name: Download current task definition`
	- Downloads your **existing** task definition from AWS and saves it as `task-def.json`
- `name: Fill in new image ID in task definition`
	- Steps
		- Takes your existing task definition (`task-def.json`)
		- Finds the container with name `ECS_CONTAINER_NAME`
		- Updates **only** the image field to point to your new image (it keeps all the other settings the same coz u might want to keep them!)
		- Creates a **new** task definition file
	- Need to do this because task definitions are immutable in AWS
- `name: Deploy Amazon ECS task definition`
	- Registers the new task definition with AWS ECS
	- Updates your ECS service to use this new task definition
	- Triggers a deployment (ECS will start new containers with the new image and stop old ones)
- `name: ECS 배포 상태 확인`