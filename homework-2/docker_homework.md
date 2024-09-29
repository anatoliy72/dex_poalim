
# Docker homework

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
docker build --no-cache -t my-nginx-app -f Dockerfile .
```
![image](https://github.com/user-attachments/assets/e8370cbd-c247-4db7-b7be-5e5db2866984)

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

- Cleared my browser's cache

![08](https://github.com/user-attachments/assets/4ed31dae-1426-4109-a091-f2848b5b349a)

#### 8.2 Verify File in Container
Next, I decided to see if the `index.html` file was actually in the container.

1. I got the container ID:
   ```bash
   docker ps
   ```

2. I entered the running container:
   ```bash
   docker exec -it a03 bash
   ```

3. I checked if the file was in the correct directory:
   ```bash
   ls /usr/share/nginx/html/
   ```

4. And then viewed the content of `index.html`:
   ```bash
   cat /usr/share/nginx/html/index.html
   ```
![image](https://github.com/user-attachments/assets/72915bd2-7f78-46d1-861e-0d8b23f33f43)

**... and it's working!!!!!**

![image](https://github.com/user-attachments/assets/324aa2d8-967c-4568-aa3e-524c93fdef15)

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

This started both the Nginx web service and the Redis service. When I run `docker ps`, I saw both containers up and running:

```bash
docker ps
```

The output showed the web service on port `8080` and the Redis service on default port `6379`. Everything looked good.

![12](https://github.com/user-attachments/assets/56d3b30e-878a-4532-8f46-5ff3b0f30c5e)

