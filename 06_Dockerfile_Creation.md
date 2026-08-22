# 📄 Dockerfile Creation Guide

![Docker](https://img.shields.io/badge/Dockerfile-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner-brightgreen?style=for-the-badge)

A **Dockerfile** is simply a plain text file containing a step-by-step list of instructions (like a recipe). Docker reads this recipe from top to bottom to automatically build your custom Docker Image.

---

## 🧑‍🍳 An Easy-to-Understand Example

Let's look at a standard Dockerfile for a simple Python web application. Read through it—it is almost like plain English!

```dockerfile
# Step 1: Pick the base operating system and language
FROM python:3.10-slim

# Step 2: Set the folder where our app will live inside the container
WORKDIR /stanapp

# Step 3: Bring our list of dependencies into the container
COPY requirements.txt .

# Step 4: Install those dependencies
RUN pip install -r requirements.txt

# Step 5: Copy the rest of our application code into the container
COPY . .

# Step 6: Document the port the app uses
EXPOSE 5000

# Step 7: Define the command to start the app
CMD ["python", "stanapp.py"]
🔍 Line-by-Line Breakdown
Here is exactly what each line in the example above is doing:

Line / Instruction,What it does
FROM python:3.10-slim,"The Starting Point: Every Dockerfile must start with a FROM instruction. It pulls a pre-existing base image (here, a lightweight version of Linux with Python 3.10 pre-installed)."
WORKDIR /app,The Workspace: Creates a directory named /app inside the container and navigates into it. All following commands will happen inside this folder.
COPY requirements.txt .,The Transfer: Copies the requirements.txt file from your local computer into the current working directory (.) of the container.
RUN pip install...,"The Builder: Executes a command during the build process. Here, it tells the container to install the Python packages listed in the text file."
COPY . .,The Final Copy: Copies all remaining files from your local project folder into the container's /app folder.
EXPOSE 5000,The Signpost: Tells Docker that the container will listen on port 5000. (Note: This doesn't actually publish the port; it acts as documentation for whoever runs the container).
"CMD [""python"", ""app.py""]","The Trigger: The default command that executes when the container starts running. (Unlike RUN, which happens while the image is being built)."

📚 Dictionary of Dockerfile Instructions
Here are the most common instructions you will use when writing your own Dockerfiles:

Instruction,Input Details & Usage,Example
FROM,Takes the name of a base image and a tag. Always the first instruction.,FROM node:18-alpine
WORKDIR,Takes an absolute or relative path inside the container.,WORKDIR /usr/src/app
COPY,Takes a source path (your machine) and a destination path (the container).,COPY package.json /app
ADD,"Similar to COPY, but can also extract .tar files or download files from a URL. (Use COPY unless you specifically need these features).",ADD https://example.com/file.txt .
RUN,Takes Linux commands to execute during the image build.,RUN apt-get update && apt-get install curl
ENV,Takes a key-value pair to set system environment variables inside the container.,ENV NODE_ENV=production
EXPOSE,Takes a port number. Acts as documentation for networking.,EXPOSE 80 443
CMD,Takes an array of strings representing the executable and its arguments. Provides the default command for an executing container.,"CMD [""npm"", ""start""]"
ENTRYPOINT,"Similar to CMD, but much harder to override from the command line. Often used when the container is designed to run as an executable.","ENTRYPOINT [""nginx"", ""-g"", ""daemon off;""]"

✨ 3 Golden Rules for Dockerfiles
[!TIP]

Keep it small: Use lightweight base images (like -alpine or -slim) to make your image download faster and reduce security risks.

Order matters: Docker caches each step. Put things that change rarely (like installing dependencies) at the top, and things that change often (like your source code) at the bottom to speed up build times.

One app per container: A container should only have one primary responsibility (e.g., run the web server OR run the database, but not both in the same file).

