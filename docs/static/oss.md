# OSS文件上传

将本地文件上传到服务器是软件系统经常会遇到的需求，例如：设置用户头像，上传Excel报表等等；涉及到网络访问性能、存储空间等因素的考虑，通常我们会选择使用第三方的对象存储服务，本类库将比较常用的第三方对象存储服务进行了封装整合，让用户能够使用一套API将文件上传到不同的对象存储服务上，方便了用户使用。

目前，类库支持了七牛、阿里云、腾讯3种对象存储服务。

```c#
public enum OssProvider
{
    Unset,
    Qiniu,		// 七牛
    Alibaba,	// 阿里云
    Tencent		// 腾讯云
}
```

在使用过程中，根据需要使用的OSS存储服务获取OSS对象，即可调用方法进行文件上传，下面将逐步说明。

## 一、获取OSS对象

首先，根据我们准备使用的OSS存储服务类型，获取对应的OSS操作对象。

```c#
IOssProvider oss = Xmtool.Oss(OssProvider.Qiniu);
// TODO
```

## 二、初始化配置

OSS操作对象，必须进行初始化配置才能进行文件上传操作，不同的存储服务类型配置参数略有不同，下面将一一说明。

### 1. 七牛存储服务初始化配置

```c#
IOssProvider oss = Xmtool.Oss(OssProvider.Qiniu);
oss.Config("替换成自己的AppKey", "替换成自己的AppSecret", "替换成自己的域名地址");
// TODO
```

### 2.阿里云存储服务初始化配置

   ```c#
   IOssProvider oss = Xmtool.Oss(OssProvider.Alibaba);
   oss.Config("替换成自己的AppKey", "替换成自己的AppSecret", "替换成自己的域名地址", "替换成存储桶对应的EndPoint地址");
   // TODO
   ```

### 3.腾讯云存储服务初始化配置

```c#
IOssProvider oss = Xmtool.Oss(OssProvider.Tencent);
oss.Config("替换成自己的AppId", "替换成自己的SecretId", "替换成自己的SecretKey", "替换成自己的域名地址", "替换成存储桶对应的Region简称");
// TODO
```

## 三、上传本地文件

经过上一步的初始化配制后，就可以进行文件上传操作了；类库支持文件路径和文件流两种上传方式。

通过文件路径上传，方法如下：

```c#
IOssProvider oss = Xmtool.Oss(OssProvider.Qiniu);
oss.Config("替换成自己的AppKey", "替换成自己的AppSecret", "替换成自己的域名地址");
string url = oss.UploadFile("替换成自己的存储桶名称", "要上传的本地文件路径", "上传到服务器的文件key，如果不传将随机生成");
// 返回的url即是上传成功后的文件访问地址，是否可公开访问根据存储桶的访问权限设定。
// TODO 
```

## 四、上传文件流

通过文件流上传，方法如下：

```c#
IOssProvider oss = Xmtool.Oss(OssProvider.Qiniu);
oss.Config("替换成自己的AppKey", "替换成自己的AppSecret", "替换成自己的域名地址");
using (FileStream stream = new FileStream("要上传的本地文件路径", FileMode.Open))
{
    string url = oss.UploadStream("替换成自己的存储桶名称", stream, "上传到服务器的文件key，如果不传将随机生成");
	// TODO
}
```

## 五、设置上传文件ContentType

在上传文件时，如果不指定ContentType，除七牛会自动判断上传文件类型外，阿里云和腾讯云都会将文件默认成二进制流文件，这在后面的访问过程中会影响文件的预览等操作。因此，我们可以在上传时明确指定文件的类型。

```c#
IOssProvider oss = Xmtool.Oss(OssProvider.Qiniu);
oss.Config("替换成自己的AppKey", "替换成自己的AppSecret", "替换成自己的域名地址");
string url = oss.SetContentType("image/png")
    .UploadFile("替换成自己的存储桶名称", "要上传的本地文件路径", "上传到服务器的文件key，如果不传将随机生成");
// TODO 
```

## 六、是否使用HTTPS

```c#
IOssProvider oss = Xmtool.Oss(OssProvider.Qiniu);
oss.Config("替换成自己的AppKey", "替换成自己的AppSecret", "替换成自己的域名地址");
oss.SetUseHttps(true);
// TODO
```

