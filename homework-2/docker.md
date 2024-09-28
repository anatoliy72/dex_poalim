
# Docker

Let me tell you how it didn’t work out for me.

## Steps:

### Step 1: Create Your Project Directory
So, I started by creating a directory for my project where I could store all the files I needed.

```bash
mkdir homework-2
cd homework-2
```

### Step 2: Create `Dockerfile`
Then, I created a `Dockerfile` in that directory. This file is supposed to tell Docker how to set up the container.

```dockerfile
# Use the official nginx image as the base image
FROM nginx:latest

# Copy your custom HTML file into the container
COPY index.html /usr/share/nginx/html/

# Expose port 80 (this is the default port that Nginx listens on)
EXPOSE 80
```

### Step 3: Create `index.html`
I created an `index.html` file with some simple content that Nginx should serve.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Docker Homework</title>
</head>
<body>
    <h1>Do I need this?</h1>
</body>
</html>
```

### Step 4: Build the Docker Image
To build the Docker image, I ran this command in my project directory:

```bash
docker build -t my-nginx-app .
```

![03](https://github.com/user-attachments/assets/940f44dc-960d-4864-a856-7fc3b37b7167)

### Step 5: Run the Docker Container
Once the image was built, I ran the container and mapped it to port `8080` on my local machine:

```bash
docker run -dp 8080:80 my-nginx-app
```

### Step 6: Verify the Running Container
I used the following command to check if the container was running:

```bash
docker ps
```

It looked like everything was set up correctly; my container was running, and the ports were mapped as expected (`8080:80`).

### Step 7: Open the Web Application in Your Browser
I opened my web browser and navigated to:

```
http://localhost:8080
```

At this point, I expected to see my `index.html` content, but...

![07](https://github.com/user-attachments/assets/6bf7a6ef-9e0f-4d74-8bfa-95fdf0837be9)

### Step 8: Troubleshooting

It didn’t work. Instead of my custom HTML, I saw the default Nginx welcome page. So, I started troubleshooting.

#### 8.1 Clear Browser Cache
Maybe it was the browser cache showing the default page. I tried clearing the cache:

- Cleared my browser's cache and tried again.
![08](https://github.com/user-attachments/assets/4ed31dae-1426-4109-a091-f2848b5b349a)

But that didn’t fix it.

#### 8.2 Rebuild Docker Image Without Cache
Then, I thought maybe Docker was using cached layers, so I rebuilt the image without cache:

```bash
docker build --no-cache -t my-nginx-app .
```

And ran the container again:

```bash
docker run -dp 8080:80 my-nginx-app
```
![05](https://github.com/user-attachments/assets/83a08ed0-bdd1-4caa-a581-e903ea32fed1)


#### 8.3 Verify File in Container
Next, I decided to see if the `index.html` file was actually in the container.

1. I got the container ID:
   ```bash
   docker ps
   ```

2. I entered the running container:
   ```bash
   docker exec -it <container_id> bash
   ```

3. I checked if the file was in the correct directory:
   ```bash
   ls /usr/share/nginx/html/
   ```

4. And then viewed the content of `index.html`:
   ```bash
   cat /usr/share/nginx/html/index.html
   ```
![06](https://github.com/user-attachments/assets/520fee8f-36fb-418b-838e-d6a821016d1d)

Everything seemed right in the container, but...

**Oops... and it's still not working.**

![10](https://github.com/user-attachments/assets/20a1042b-ede9-4603-a176-bbea4201f075)

---

# Docker Compose Setup:

I continued with the `docker-compose.yml` file configuration, which set up two services: my new Nginx web service and a Redis service.

Here’s the content of my `docker-compose.yml`:

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8080:80"
    depends_on:
      - redis
  redis:
    image: "redis:alpine"
    ports:
      - "6379:6379"
```

### Running Docker Compose

With everything set up, I executed the following command to start both services:

```bash
docker-compose up -d
```

This started both the Nginx web service and the Redis service. When I ran `docker ps`, I saw both containers up and running:

```bash
docker ps
```

The output showed the web service on port `8080` and the Redis service on `6379`. Everything looked good.

![12](https://github.com/user-attachments/assets/56d3b30e-878a-4532-8f46-5ff3b0f30c5e)

## Conclusion
This was supposed to be a simple task: setting up an Nginx web server using Docker. 
But despite following the steps and troubleshooting, it didn’t work out for me. Clearing the cache and rebuilding without cache didn’t fix the issue. If you try this, I hope you’ll have better luck!

---
