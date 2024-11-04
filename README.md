# Step 1: Install Nginx with RTMP Module



1\. Connect to Your Server:
---------------------------

```tsx
SSH username@ip-address
```


2\. Install Required Packages:



```txt
sudo apt update
```

```txt
sudo apt install -y build-essential libssl-dev libpcre3 libpcre3-dev libcurl4-openssl-dev libgeoip-dev libxslt1-dev git
```


3. Download and Install Nginx with RTMP:

Clone the Nginx RTMP module:
----------------------------

```tsx
git clone https://github.com/arut/nginx-rtmp-module.git
```

## Download Nginx source code (replace 1.22.0 with the desired version):

```tsx
wget http://nginx.org/download/nginx-1.22.0.tar.gz
```

```tsx
tar -zxvf nginx-1.22.0.tar.gz
```

Compile and install Nginx with the RTMP module : 



```tsx
cd nginx-1.22.0
```

```tsx
./configure --with-http_ssl_module --add-module=../nginx-rtmp-module
```

```tsx
make
```

```tsx
sudo make install
```

## 4. Start Nginx:

```txt
sudo /usr/local/nginx/sbin/nginx
```

# Step 2: Configure Nginx for RTMP Streaming



## 1. Edit Nginx Configuration: Open the Nginx configuration file:

```txt
 sudo nano /usr/local/nginx/conf/nginx.conf
```


## 2. Add the RTMP Configuration: Add the following configuration within the http block (or create a new rtmp block if it doesn't exist):12211



```tsx
rtmp {
    server {
        listen 1935; # RTMP server port
        chunk_size 4096;

        application live {
            live on;
            record off;

            # Define your streaming outputs here
            push rtmps:<URL>
            push rtmps:<URL>
            push rtmps:<URL>
        }
    }
}

```

## Example :

```txt
  push rtmps://dc5-1.rtmp.t.me/s/24223377358:Zrtd5agh72iz_AlJspjkExA;
```

## 3. Save and Exit: Save the file and exit the editor.
## 4. Restart Nginx:

```txt 
sudo /usr/local/nginx/sbin/nginx -s reload
```

# Step 3: Streaming to Telegram RTMP Servers

### You’ll stream to Telegram RTMP servers using the following command in your terminal or script. Here’s how you can do it with FFmpeg:

## 1. Install FFmpeg: If you haven't already, install FFmpeg:

```txt
 sudo apt install ffmpeg
```

## Stream Using FFmpeg: Use the following commands to stream to the Telegram servers you provided. You will need to run each command for every stream:

```txt
 ffmpeg -re -i input\_video.mp4 -c\:v copy -c\:a aac -f flv rtmps://dc5-1.rtmp.t.me/s/<Your-Stream-Key>
```

# Step 4: Verifying Your Streams

- Check Nginx Logs: You can monitor the logs to see if the streams are being received correctly. The logs are usually located in /usr/local/nginx/logs/error.log or can be configured in your Nginx config.
- Test Streaming: Ensure that your videos are streaming correctly by checking them in your Telegram groups.

## Additional Notes

- Make sure your server's firewall allows traffic on port 1935 for RTMP.

- Adjust the FFmpeg commands as needed for your input video, bitrate, and encoding settings.

