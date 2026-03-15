create a new git repo , give an unique name and description and click create repo




Copy the repo link

Navigate to your local folder where collections present, e.g: C:\Users\satya\OneDrive\Documents\Jan2025_PostamanCollections
open new cmd and cd to “C:\Users\satya\OneDrive\Documents\Jan2025_PostamanCollections”

https://github.com/green2043/APIPostmanCollection.git
Then run these command only once
git init (to initialize git)
git remote add origin https://github.com/green2043/APIPostmanCollection.git 
git status 
Then you will get something like this:

red ones are the untracked files for git, i.e - files are in local workspace
Now run these commands on regular basis:
git add . (. means all, i.e - all files will be added/copied to staging area)

git status ( will show current status of files- all red files will come in green- ready to be committed)


git commit -m “adding my all api collections to git”

Current location of files are in local repository
Files are ready to push


NOTE: For first time git users, it will ask for token and authentication - it’s one time effort
But in my case I am already using git for so long, so not required, I have already authenticated

run below command to push:
git push origin master ( it will create a master folder in remote git repo and push all files from local repo to this branch )
I got an interesting error:

1. What the error means
Your command:
git push origin master
Git started uploading your commits to GitHub.
 Everything looked fine until GitHub checked the contents of your commit.
Then GitHub stopped the push with this message:
GH013: Repository rule violations found
Push cannot contain secrets
And it specifically shows this:
Postman API Key
path: API_KEY.txt:1
What this really means is simple:
You accidentally pushed a secret key (API key) into your repository.
GitHub has a security feature called Secret Scanning + Push Protection.
 When it detects things like:
API keys


tokens


passwords


private credentials


it blocks the push immediately to prevent security leaks.
So GitHub rejected your push to protect you.

2. Exactly where the problem is
GitHub tells you clearly:
path: API_KEY.txt:1
Meaning:
Inside this file:
API_KEY.txt
Line 1 contains something that looks like a Postman API Key.
Example:
PMAK-xxxxxxxxxxxxxxxxxxxxxxxx
or some token-like string.
GitHub detected that pattern and blocked it.

3. Why GitHub blocks this
If an API key gets pushed to a public repo:
Anyone on the internet can use it.
That can lead to:
API abuse


account compromise


data theft


billing charges


So GitHub stops the push.

Fix the issue:
Step 1 — Add the file to .gitignore
Open your project folder.
Open or create a file called:
.gitignore
type this line inside it:
API_KEY.txt
Save the file.
What this does:
 Git will ignore this file from now on.




Step 2 — Remove the file from Git tracking
Run this command in your terminal:
git rm --cached "API KEY.txt"
Important:
 --cached means
Remove from Git, but keep the file on your computer.
So your key stays safe locally.

Step 3 — Commit the change
Run:
git add .
git commit -m "Ignore API key file"

Step 4 — Push again
Now push normally:
git push origin master
Now GitHub will not push API_KEY.txt.

What the final result will be
Your repo will contain:
.gitignore
postman_collection.json
other files...
But NOT
API_KEY.txt
That file stays only on your computer.

GitHub already gave you a special link in the error:

https://github.com/green2043/APIPostmanCollection/security/secret-scanning/unblock-secret/...
Do this:

Open that link in your browser.

GitHub will show the detected secret.

Click "Allow secret" / "Unblock push".

Confirm.

This tells GitHub:

Yes, I know this key exists. Allow this push.

After that run again:

git push origin master

and it will succeed.

Important (security)

Since your key was exposed in Git history, you should regenerate the Postman API key in Postman.

Old key = potentially compromised.

Best practice going forward

Keep secrets outside Git.

Example:

.gitignore

API KEY.txt
.env

Example .env:

POSTMAN_API_KEY=xxxxx

This is how most automation projects handle credentials.

If you want, I can also show you a very clean structure for storing Postman collections + environments in GitHub that many QA automation engineers use. It makes API projects look very professional.

what to select ?

You are on the GitHub push protection screen. GitHub detected a Postman API key in your commit history and is asking whether to allow it.

Here’s the important part:
Since you already removed the file and added .gitignore, you just need to allow this push once so GitHub stops blocking it.

What to select

Choose:

It's used in tests

Then click the button:

Allow me to expose this secret

That will unblock your push.

Why this option is correct

Your situation:

You removed API KEY.txt

The key only exists in old commit history

You just want GitHub to stop blocking the push

Selecting It's used in tests tells GitHub:

This key is not a real production secret.

After clicking allow

Go back to your terminal and run:

git push origin master

Your push should succeed now.

One important thing

Since the key appeared in Git history, it is good practice to regenerate the API key in Postman later.

Old key → revoke
New key → keep locally (not in Git).

Quick tip for future projects

Instead of storing keys like:

API KEY.txt

Use a .env file and ignore it:

.env

And inside:

POSTMAN_API_KEY=your_key_here

Then add this to .gitignore:

.env

This is the standard way developers store API keys.





