# 文档库的版本控制——SPFileVersion
   	作者：柒月

## 描述
SharePoint文档库开启`版本控制`后，可对文档进行版本管理。
通过以下代码可以获取各版本信息。
```
SPFile file = 获取SPFile;
foreach (SPFileVersion version in file.Versions)//SPFile.Versions返回的类型是SPFileVersionCollection
{
    string comment = version.CheckInComment;//签入日志
    string versionNo = version.VersionLabel;//版本号
    SPFile versionFile = version.File;
    byte[] versionFileByte = versionFile.OpenBinary();
    //TODO 导出历史版本文件
}
```
慢着，代码执行后，本应该导出各版本的文件，现在却导出了N份最新版本的文件。有什么奇怪的东西混进去了？
## 思考
`SPFileVersion.File`是一个`SPFile`对象，若它表示某历史版本的文件，那么它所对应的`SPFile.Versions`又是什么？历史版本文件的历史版本文件集合？好矛盾，好纠结，显然错误得很彻底。

再仔细瞅瞅`SPFileVersion.File`的Summary:`Gets the parent file for the version.` 原来这是用来获取父文件(Release版本文件)的。而`SPFileVersion.OpenBinary() Returns a byte array that contains the file version.`才是用来获取历史版本文件的。
## 有码有真相
反编译一下`Microsoft.SharePoint.dll`找到`SPFileVersion.File`属性   :D
```
public SPFile File
{
    get
    {
        //m_FileVersions为SPFileVersionCollection
        return this.m_FileVersions.File;
    }
}

```
顺道发现了另一个有意思的东西`SPFileVersion.VersionLabel`，版本号，值为`1.0`  `2.2`  `3.12` 等。小数点前的数字是主要版本号；小数点后面的数字为次要版本号。
```
public string VersionLabel
{
    get
    {
        //this.ID为内部版本号，每产生一个主要版本this.ID+=512，每产生一个次要版本this.ID+=1
        //num为主要版本号，SPFile.MAX_MINORVERSION是readonly的，值为512
        int num = this.ID / SPFile.MAX_MINORVERSION;
        //num2为次要版本号
        int num2 = this.ID % SPFile.MAX_MINORVERSION;
        return (num.ToString() + '.' + num2.ToString());
    }
}

```
这样看来，一个文件最多有511个次要版本，第512个次要版本自动发布成主要版本咯？  XDD

