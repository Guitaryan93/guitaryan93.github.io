# On the road to Senior Developer...

Hello! I'm Ryan. A software developer. This website is dedicated to documenting my journey from absolute noob to god tier Senior Developer.

The idea for this site came from a few different sources:
- Many influential programmers telling the masses to "work in the open"
- Job applications having that one little "Personal Website" field that I have nothing to put in...
- Getting abruptly knocked out of my blissfully ignorant bubble of stale programming skills by an AI

That's right! I got cussed out by ChatGPT. That people pleasing, can't say no, em-dash loving robot called me out on my Python code I submitted for a technical programming interview question.

> "My overall assessment is that this is a reasonably good junior-level solution, but it isn't quite production-quality Python."
> -- Chat Gippity

So I decided that it is time to get good at this programming thing so I can pass that interview question and show ChatGPT who's boss.

# The Plan
I am going to use AI to accelerate the learning process by turning ChatGPT into my tutor. ChatGPT will provide me with projects and the requirements for each project. Then I will design and build the project, documenting the process along the way via blog posts to this site.

Once the project, or current development cycle for the project, has been completed I will have ChatGPT review my project structure, code, tests, design decisions, etc... and have them quiz me on my choices as well as point out ways I could improve upon my work. Then I will make the changes that I think make sense and submit for another code review. Rinse and repeat until I am happy with the project.

# Where it all began
Before I get started on the road to Senior Developer let me set the scene for you with some information about myself.

I began to learn programming in 2020 during the global pandemic. My job at that time was unaffected regarding going into the store for 8 hours a day and working my shift before heading home. So, I worked away at it a little each evening by watching YouTube videos, reading articles and documentation, and writing a loooooot of bad Python programs.

After two years of slogging it out with Python, HTML, CSS, Javascript and some basic scripting, I managed to get a job in software support for a legacy point-of-sale system. I got some amazing experience cracking bug reports and passing on my findings to the development team.

After a while of snooping around the codebase at work I started experimenting with the language. Have you ever heard of Progress ABL? Yeah... me neither! But that is how legacy we are talking. The system is older than I am and still adding new features to this day. As annoying as legacy codebases can be, I have serious respect for the people that wrote it (without Stack Overflow, dun, dun, DUUUUN!) and have maintained it for all these years.

In my free time I hacked away at a Wordle clone in Progress and then passed it on to the head of the software team to see what they thought. Luckily, they have a sense of humor and added it to the point-of-sale system under a hidden menu. Then they added me to the development team, and the rest is history!

So here I am. A mid-tier Python developer. One of the top 10 Progress developers in Canada (there are only 8 of us total...). And I have a chip on my shoulder that I got from my soon-to-be AI overlord.

Now, let's get programming!

# Trigger Warning
For completeness sake, here is the code ChatGPT reviewed that started me on this road. I am hoping that as I progress in my knowledge and skills that seeing this will serve as a reminder for how far I have come. For anyone that is reading this and thinks that this code is way beyond their current abilities, please remember that we have ALL been there. Every programmer started with `print("Hello, World)` and got that error about the missing `"` at the end of the Hello, World string... And anyone reading this that thinks my code sucks. Well, I'm coming for you!

The prompt provided by the company was to take a published Google Doc with an HTML table of coordinates and characters in it, parse it, and then print it to the screen so that the ASCII art lettering would appear correctly. One of the tricks to this was inverting the printing of the y coordinates. The character grid is designed with x,y = 0,0 to be in the bottom left-hand corner.

```python
import requests
from bs4 import BeautifulSoup


def print_graphic(url):

    # wrap requests call in try/except block to handle errors with request
    try:
        response = requests.get(url)
        response.raise_for_status()
    except requests.exceptions.RequestException as e:
        print(f"An error occurred: {e}")
        return

    soup = BeautifulSoup(response.text, "html.parser")

    # get coordinates and character string from HTML table cells
    cell_list = [table_data.get_text() for table_data in soup.find_all("td")]

    # exit if there is no table of coordinates found
    if len(cell_list) == 0:
        print("No HTML table of coordinates found...")
        return

    # group coordinates into lists [x, char, y]
    coords_list = [cell_list[i : i + 3] for i in range(0, len(cell_list), 3)]

    # find the maximum x and y values to get grid size
    max_x, max_y = 0, 0
    for coord in coords_list:
        if coord[0].isdigit() and coord[2].isdigit():
            if int(coord[0]) > max_x:
                max_x = int(coord[0])
            if int(coord[2]) > max_y:
                max_y = int(coord[2])

    # loop over grid and print characters at each coordinate
    for y in range(max_y, -1, -1):
        for x in range(0, max_x + 1):
            character = " "

            # see if a matching coordinate exists and grab the character value
            for coord in coords_list:
                if coord[0] == str(x) and coord[2] == str(y):
                    character = coord[1]
                    break

            print(character, end="")

        print()  # newline for each "y"


# Below this line is for testing and was not included in the test answer
test_URL = "https://docs.google.com/document/d/e/2PACX-1vTMOmshQe8YvaRXi6gEPKKlsC6UpFJSMAk4mQjLm_u1gmHdVVTaeh7nBNFBRlui0sTZ-snGwZM4DBCT/pub"
test_URL2 = "https://docs.google.com/document/d/e/2PACX-1vSvM5gDlNvt7npYHhp_XfsJvuntUhq184By5xO_pA4b_gCWeXb6dM6ZxwN8rE6S4ghUsCj2VKR21oEP/pub"


print_graphic(test_URL)
print()
print()
print_graphic(test_URL2)
```
