# Proxy for Docker and other Stuffs 
### [Freedom of Developers](https://github.com/freedomofdevelopers/fod#docker)

- [Android Studio](https://github.com/freedomofdevelopers/fod#android-studio)
- [Gradl](https://github.com/freedomofdevelopers/fod#gradle)
- [FireFox](https://github.com/freedomofdevelopers/fod#%D9%81%D8%A7%DB%8C%D8%B1%D9%81%D8%A7%DA%A9%D8%B3)
- [Chrome](https://github.com/freedomofdevelopers/fod#chrome)
- [Proxifier](https://github.com/freedomofdevelopers/fod#proxifier)
- [Git](https://github.com/freedomofdevelopers/fod#git)
- [Mercurial](https://github.com/freedomofdevelopers/fod#mercurial)
- [CLI](https://github.com/freedomofdevelopers/fod#%D8%AE%D8%B7-%D9%81%D8%B1%D9%85%D8%A7%D9%86-%D9%84%DB%8C%D9%86%D9%88%DA%A9%D8%B3)
- [npm](https://github.com/freedomofdevelopers/fod#npm)
- [Docker](https://github.com/freedomofdevelopers/fod#docker)

</br></br></br>


# Docker 

1. `mkdir -p /etc/systemd/system/docker.service.d`

2. `vi /etc/systemd/system/docker.service.d/http-proxy.conf`
   
   > Add These Lines to http-proxy.conf
    ```
    [Service]
    Environment="HTTPS_PROXY=http://fodev.org:8118"
    Environment="HTTP_PROXY=http://fodev.org:8118"
    Environment="NO_PROXY=<Private Registry>"
    ```
   > With Socks5 

    ```
    [Service]
    Environment="HTTPS_PROXY=socks5://<Proxy-IP>:<Proxy-Port>"
    ```

3. `systemctl daemon-reload`
4. `systemctl restart docker`

</br></br>

# Git
> Just Invoke These Commands:

   ```
   git config --global http.proxy fodev.org:8118
   ```

   ```
   git config --global https.proxy fodev.org:8118
   ```