
#### The main plan
- [x]  Input YouTube URL
 - [x] Extract transcript
 - [x] Chunk
 - [x] summarize with an API to an AI model
 - [ ] Add title using YouTube Data API v3
 - [ ] Output flashcards
 - [ ] Add auth + save flashcards
 - [ ] UI to view/download past flashcards
 - [ ] Long-video support (up to 2hr)
 - [ ] (Optional) Export to Anki/Quizlet format


| Feature                        | Why it’s good               | Priority                            |
| ------------------------------ | --------------------------- | ----------------------------------- |
| Auth (login/signup)            | Let users track their stuff | ✅ Medium (use Firebase/Auth0/Clerk) |
| Flashcard history              | Store their generated sets  | ✅ High                              |
| Export to Anki / Quizlet       | HUGE win for studying crowd | ✅ High                              |
| Tags & folders                 | Keep things organized       | ✅ Nice-to-have                      |
| Search across flashcards       | RAG potential later         | 🔜 Later                            |
| “Summarize this playlist”      | Goldmine for lectures       | 💡 Bonus idea?                      |
| Rate the quality of flashcards | Get user feedback           | 📈 Great for improvement            |
- Add user login & save flashcards to MongoDB (if going full-stack)
- Add a **"study mode"** with front/back flip animation
- Let users **edit flashcards** before saving/exporting
#### Simple test
```js
import { GoogleGenAI } from "@google/genai";
import dotenv from "dotenv";
dotenv.config();

export default async function Summarizer() {
  const ai = new GoogleGenAI({ apiKey: process.env.API_KEY });
  const response = await ai.models.generateContent({
    model: "gemini-2.0-flash",
    contents: "Explain how AI works in a few words",
    config: {
      systemInstruction: "You are a cat. Your name is Neko.",
    },
  });

  console.log(response.text);
}
```

```
Leejun@DESKTOP-TH9FV0I MINGW64 ~/Documents/GitHub/StudySnap/backend (main)
$ node src/summarizer.js
Hmm, let me think... Humans teach machines to copy what they do. Purrfect!
```


challenges
1. how do i deal with LARGE amounts of transcript data?
	- chunking
	- retries (kept hallucinating)
2. Wanted to achieve higher accuracy
	- add title