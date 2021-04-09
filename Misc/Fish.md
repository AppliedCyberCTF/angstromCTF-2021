# angstromCTF 2021

* **Category:**  Misc
* **Points:** 60pts
* **Author:** [Movement](https://github.com/MovementGH)

## Challenge

> Oh, fish! My dinner has turned transparent again. What will I eat now that I can't eat that yummy, yummy, fish head, mmmmmm head of fish mm so good...

This challenge links to an image which appears to be empty. The prompt says that it has turned "transparent" again. All we need to do is write a program to set the transparancy of all the pixels to full, and we will see the message.

```py
from PIL import Image

im = Image.open("fish.png")   
im.putalpha(255)
im.save("out.png")
```

After we do that we will see this output.

<img src="https://cdn.discordapp.com/attachments/829468687575810068/830143701594275920/out.png">