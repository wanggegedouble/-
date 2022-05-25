# -
server
int main()
{
    //成功返回一个用于监听的文件描述符  失败返回-1·
    int fd = socket(AF_INET,SOCK_STREAM,0);
    if(fd==-1)
    {
        perror("socket");
        return -1;
    }
    struct sockaddr_in saddr;
    //初始化saddr结构体
    saddr.sin_family=AF_INET;//ipv4
    saddr.sin_port=htons(12345);
    saddr.sin_addr.s_addr = INADDR_ANY;//IP 地址
    int ret = bind(fd,(struct sockaddr*)&saddr,sizeof(saddr));
    if(ret==-1)
    {
        perror("bind");
        return -1;
    }
    ret =listen(fd,128);
    if(ret==-1)
    {
        perror("listen");
        return -1;
    }
    struct sockaddr_in caddr;
    int addrlen = sizeof(caddr);
    int cfd = accept(fd,(struct sockaddr*)&caddr,&addrlen);
    if(cfd == -1)
    {
        perror("accpet");
        return -1;
    }
    printf("客户端的IP:%s,端口:%d\n,
         inet_ntop(AF_INET,&caddr.sin_addr.s_addr,ip,sizeof(ip)),
          ntohs(caddr.sin_port));
    while(1)
    {
        char buff[1024];
        int len = recv(cfd,buff,sizeof(buff),0);
        if(len>0)
        {
            printf("client say:%s\n",buff);
            send(cfd,buff,len,0);
        }
        else if(len ==0)
        {
            printf("客户端断开连接...\n");
            break;
        }
        else
        {
            perror("recv");
            break;
        }
    }
    close(fd);
    close(cfd);
    return 0;
}
int main()
{
     int fd = socket(AF_INET,sock_STREAM,0);
    if(fd==-1)
    {
        perror("socket");
        return -1;
    }
    struct sockaddr_in saddr;
    saddr.sin_family=AF_INET;
    saddr.sin_port=htons(12345);
    inet_pton(AF_INET,"192.168.43.210",&saddr.sin_addr.s_addr);
    int ret connect(fd,(steuct sockaddr*)&saddr,sizeof(saddr));
    if(ret == -1)
    {
        perror("connect");
        return -1;
    }
    int number=0;
    while(1)
    {
        char buff[1024];
        sprintf(buff,"你好，hello word,%d...\n",number++);
        send(fd,buff,strlen(buff)+1,0);
        memset(buff,0,sizeof(buff));
        int len = recv(cfd,buff,sizeof(buff),0);
        if(len>0)
        {
            peinrf("client say:%s",buff);
        }
        else if(len==0)
        {
            printf("服务器断开连接..\n");
            break;
        }
        else
        {
            perror("recv");
            break;
        }
    }
    close(fd);
    return 0;
}
