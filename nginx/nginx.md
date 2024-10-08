## nginx在docker下的部署

### 不带配置文件的映射
```bash
docker run \
--restart always \
--name Nginx \
-d \
-p 80:80 \
nginx
```
### 带配置文件的映射
将容器中Nginx的配置文件映射到宿主机下某个文件和目录，后续修改Nginx的配置就直接修改宿主机下映射目录下的配置文件，而不需要进入到容器中修改了。
```bash
# 简易版
docker run \
--restart always \
--name Nginx \
-d \
-v /dev-ops/nginx/html:/usr/share/nginx/html \
-v /dev-ops/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-p 80:80 \
nginx

# 完整版
docker run \
--name Nginx \
-p 443:443 -p 80:80 \
-v /dev-ops/nginx/logs:/var/log/nginx \
-v /dev-ops/nginx/html:/usr/share/nginx/html \
-v /dev-ops/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /dev-ops/nginx/conf/conf.d:/etc/nginx/conf.d \
-v /dev-ops/nginx/ssl:/etc/nginx/ssl/  \
--privileged=true -d --restart=always nginx

# -v 是映射配置
```

## 配置信息
配置文件映射的目录
```path
/dev-ops/nginx/...
```

映射目录中的Nginx配置文件初始为容器中默认的文件，下面的操作是容器复制文件的命令
* 将docker 容器中的Nginx配置复制到映射文件目录下
```bash
# 拷贝/etc/nginx/nginx.conf
docker container cp Nginx:/etc/nginx/nginx.conf /dev-ops/nginx/conf
# 拷贝/etc/nginx/conf.d/default.conf
mkdir /dev-ops/nginx/conf/conf.d
docker container cp Nginx:/etc/nginx/conf.d/default.conf /dev-ops/nginx/conf/conf.d/default.conf
# 拷贝/usr/share/nginx/html/index.html
docker container cp Nginx:/usr/share/nginx/html/index.html /dev-ops/nginx/html
```

