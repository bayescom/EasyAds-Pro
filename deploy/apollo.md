# 运行环境
## 环境说明
本wiki只针对`ubuntu(24.04 LTS)`进行验证，其他系统请自行测试。
## nodejs
- node: v16.20.2
- npm: v8.19.4
  参考[官方文档](https://nodejs.org/en/download/current)
```bash
# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
# Download and install Node.js:
nvm install 16
# Verify the Node.js version:
node -v # Should print "v16.20.2".
nvm current # Should print "v16.20.2".
# Verify npm version:
npm -v # Should print "8.19.4".
```
## nginx(可选)
- nginx: latest
  参考[官方文档](https://nginx.org/en/linux_packages.html#Ubuntu)
```bash
# Install the prerequisites:
sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring
# Import an official nginx signing key so apt could verify the packages authenticity. Fetch the key:

curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
# Verify that the downloaded file contains the proper key:

gpg --dry-run --quiet --no-keyring --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg
# The output should contain the full fingerprint 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 as follows:

pub   rsa2048 2011-08-19 [SC] [expires: 2027-05-24]
      573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
uid                      nginx signing key <signing-key@nginx.com>
# Note that the output can contain other keys used to sign the packages.
# To set up the apt repository for stable nginx packages, run the following command:
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
# Set up repository pinning to prefer our packages over distribution-provided ones:
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
#To install nginx, run the following commands:
sudo apt update
sudo apt install nginx
```
---

# 编译安装
## 1. 修改配置中的Luna链接
配置文件为`src/config.ts`，将`luna`更改为自己的`luna`服务域名
```typescript
export default {
  default: {
    luna: 'http://luna.yourdomain.com/Luna'
  },
  production: {
    luna: 'http://luna.yourdomain.com/Luna',
  }
};
```
## 2. 编译部署
```bash
# 安装依赖package
npm install
# 执行构建
cd src && npm run build
# 启动
npm start 
```
## 3. nginx托管(可选）
配置参考
```
server { 
    listen                    80;
    # 更新为自己的域名
    server_name  apollo.yourdomain.com;
    # 编译生成的静态文件目录
    root /var/www/html/build;

    location / {
        try_files $uri $uri/ /index.html;
        if ($request_filename ~* .*\.(?:htm|html)$)
        {
            add_header Cache-Control "private, no-store, no-cache, must-revalidate, proxy-revalidate";
        }
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

}
```
