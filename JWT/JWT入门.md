# 1、什么是JWT

Json Web Token（JWT），是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准（RFC 7519）.该Token被设计为紧凑且安全的，特别适合于分布式站点的单点登录（SSO）场景。JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源，也可以增加一些额外的其他业务所必须的声明信息，该token也可直接被用于认证，也可被加密。

# 2、JWT的流程

- 用户使用用户名密码来请求服务器
- 服务器进行验证用户的信息
- 服务器通过验证发送给用户一个token
- 客户端存储token，并在每次请求时附送上这个token值
- 服务端验证token值，并返回数据

# 3、JWT组成

JWT是一个字符串，由三部分组成，**头部、载荷和签名**

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

## 头部（Header）

Header部分是一个Json对象，描述JWT的元数据，通常是下面这个样子：

```json
{
	"typ":"JWT",
    "alg":"HS256"
}
```

* `typ`表示这个token的类型
* `alg`表示签名的算法，默认是HMAC SHA256（写成HS256）

需要对Header进行Base64编码转换成字符串，如下所示：

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
```

## 载荷（Payload）

Payload部分也是一个JSON对象，用来存放实际需要传递的数据。JWT官方定义了7个字段。

* iss：签发人
* exp：过期时间
* sub：jwt面向的用户
* aud：接受jwt的一方
* nbf：生效时间
* iat：签发时间
* jti：编号

除了以上字段，还可以自定义字段，如：

```json
{
    "sub":"12131323423",
    "name":"张三"
    "admin":true
}
```

**注意：载荷的数据是不加密的，不建议在部分加入敏感信息**

## 签名（Signature）

Signature部分是对前两部分的签名，防止数据篡改。签证信息由三部分组成：

* header（base64后的）
* payload（base64后的）
* secret

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

# 4、使用方式

JWT放在HTTP请求头的`Authorization`中

```
Authorization:Bearer<token>
```

# 5、SpringBoot+JWT

