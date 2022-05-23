```bash

创建自签名证书

1、生成私钥
$ openssl genrsa -out ca.key 2048

2、生成公钥
$ openssl req -new -key ca.key -out ca.csr

Enter pass phrase for ca.key:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:CN
State or Province Name (full name) []:zhejianghangzhou
Locality Name (eg, city) []:zhejianghangzhou
Organization Name (eg, company) []:bn.com
Organizational Unit Name (eg, section) []:section 3
Common Name (eg, fully qualified host name) []:xjy.bn.com        
Email Address []:1129996544@qq.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:

3、生成证书
$ openssl x509 -req -sha256 -days 3650 -in ca.csr -signkey ca.key -out ca.crt

Signature ok
subject=/C=CN/ST=zhejianghangzhou/L=zhejianghangzhou/O=bn.com/OU=section 3/CN=xjy.bn.com/emailAddress=1129996544@qq.com
Getting Private key
Enter pass phrase for ca.key:


使用开启扩展SAN的证书
SAN(Subject Alternative Name) 是 SSL 标准 x509 中定义的一个扩展。使用了 SAN 字段的 SSL 证书，可以扩展此证书支持的域名，使得一个证书可以支持多个不同域名的解析。

新建server.conf文件
$ vim server.conf

[ req ]
default_bits       = 2048
distinguished_name = req_distinguished_name
req_extensions     = req_ext

[ req_distinguished_name ]
countryName                 = Country Name (2 letter code)
countryName_default         = CN
stateOrProvinceName         = State or Province Name (full name)
stateOrProvinceName_default = ZheJiang
localityName                = Locality Name (eg, city)
localityName_default        = HangZhou
organizationName            = Organization Name (eg, company)
organizationName_default    = Sheld
commonName                  = Common Name (e.g. server FQDN or YOUR name)
commonName_max              = 64
commonName_default          = xjy.bn.com    # 此处尤为重要，需要用该服务名字填写到客户端的代码中

[ req_ext ]
subjectAltName = @alt_names

[alt_names]
DNS.1   = xjy.bn.com # 此处尤为重要，需要用该服务名字填写到客户端的代码中
IP      = 127.0.0.1

$ openssl genrsa -out server.key 4096
$ openssl req -new -sha256 -key server.key -out server.csr -config server.conf
$ openssl x509 -req -sha256 -days 3650 -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.pem -extensions req_ext -extfile server.conf


```

