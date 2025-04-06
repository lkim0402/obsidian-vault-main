> Modules that exist online on a server
- anyone can download
- https://pypi.org/
- You can download external modules using `pip`
	- Explained here: [[Virtual environment]]
- `autopep8`
	- you can download this to clean up code (to follow conventions)
	- In terminal: `autopep8 --in-place file.py`
### Example: termcolor
- prints color to terminal output)
- help(termcolor)`: Tells us how to use `termcolor`
```sh
python3 -m pip install termcolor
```
```python
# ===== python file =====
import termcolor
print(termcolor.colored("hi", color='blue'))
```
![[Pasted image 20241011212626.png]]

### Example 2: pyfiglet
```python
from pyfiglet import figlet_format
from termcolor import colored

valid_colors = [....]

def print_art(msg, color):
	if color not in valid_colors:
		color = "magenta"
	art = figlet_format(msg)
	colored = (art, color = color)
	print(colored)

msg = input("What would you like to print? ")
color = input("What color? ")
print_art(msg, color)
```
![[Pasted image 20241011214216.png]]