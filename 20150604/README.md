# SharePoint的门票——基于声明的身份验证
   	作者：柒月

仍记得第一次创建基于声明的Web Application时的感受，好像什么都没有发生，什么都没有改变（相对于Windows经典身份验证），直到我发现SPUser.LoginName加了一串奇怪的字符：

> Windows经典身份验证： sp\spadmin

> 基于声明身份验证：    i:0#.w|sp\spadmin

WTF!前面这一小段脏兮兮的字符串是什么鬼！是我大MS用来杀死处女座的么？！
还好还好，对象模型（`SPClaimProviderManager.DecodeClaim`）能帮我找回干净的登录名，不用萌萌地去做字符串截取了:)
## 声明是什么
先想办法抓出来瞧一瞧（需引用`Microsoft.IdentityModel.dll`）

```
protected override void RenderContents(HtmlTextWriter writer)
 {
    try
    {
        //取得当前用户身份的相关信息
        IClaimsIdentity currentIdentity = System.Threading.Thread.CurrentPrincipal.Identity as IClaimsIdentity;
        writer.Write("---Subject:" + currentIdentity.Name + "<BR/>");

        foreach (Claim claim in currentIdentity.Claims)
        {
            writer.Write("   ClaimType: " + claim.ClaimType + "<BR/>");
            writer.Write("   ClaimValue: " + claim.Value + "<BR/");
            writer.Write("   ClaimValueTypes: " + claim.ValueType + "<BR/>");
            writer.Write("   Issuer: " + claim.Issuer + "<BR/");
            writer.Write("   OriginalIssuer: " + claim.OriginalIssuer + "<BR/>");
            writer.Write("   Properties: " + claim.Properties.Count.ToString() + "<BR/>");
        }
    }
    catch (Exception ex)
    {
        writer.Write("exception occurred: " + ex.Message);
    }

}
```
运行后得到了一大笔数据：
```
---Subject:0#.w|sp\spadmin
ClaimType: http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
ClaimValue: sp\spadmin
Issuer: SharePoint
Properties: 0
ClaimType: http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid
ClaimValue: S-1-5-21-184028461-172769888-2126062962-31262
Issuer: SharePoint
Properties: 0
ClaimType: http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid
ClaimValue: S-1-5-21-184028461-172769888-2126062962-513
Issuer: SharePoint
Properties: 0
......
```
观察一下以上信息：当前登录者的身份信息里，有一组声明集合（Claims），它包含了很多条声明（Claim），每条声明都描述了当前登录者的某一特性，例如登录名、邮箱、属于哪一个Domain Group 等。
也就是说，当用户完成登录动作的时候，SharePoint已经将登录者所有的（所需的，从AD取）信息都取了出来，并保存在Claims里。当SharePoint需要时（进行权限验证时），直接从Claims里获取相应的授权信息，如某SharePoint资源授权域组A可以访问，从当前登录者Claims里发现他是域组A成员，那么当前登陆者可以访问此资源，至此，登陆者就打开了SharePoint的大门。

附：为什么使用基于声明的身份验证 https://msdn.microsoft.com/en-us/library/office/ee535229(v=office.14).aspx
