# CodeInstall 接入文档

## Web 端

1. 引入 codeinstall.js

    ```
    <script src="https://cdn.jsdelivr.net/gh/codeinstallapp/codeinstall@main/codeinstall.js"></script>
    ```

2. 事例

    ```
    <html>
        <head>
            <script src="https://cdn.jsdelivr.net/gh/codeinstallapp/codeinstall@main/codeinstall.js"></script>
        </head>
        <body>
            <button id="download" style="font-size:100px;">Loading...</button>
        </body>
        <script>
           async function a(){
                try{
                    var link = await CodeInstall('CodeInstall提供给您的APPID');
                    document.querySelector('#download').innerText = "Download";
                    document.querySelector('#download').addEventListener('click', ()=>{
                        location.href = link;
                    });
                }catch(e){
                    alert(`${e}`);
                }
            }
            a();
        </script>
    </html>
    ```

## Android 端

1. 下载 SDK：[codeinstallsdk.aar](codeinstallsdk.aar)

    注意：targetSdkVersion 的值必须小于等于 29

2. 事例

    ```
    import codeinstallsdk.Codeinstallsdk;
    import android.os.Build;

    ...

    // 【重要】
    // 1. 调用 get 方法，如果 code 不为空字符串，我们即可能（会尽量去重）认为是【一次有效匹配安装】；如果 code 为空字符串，我们即认为是【一次有效未匹配安装】（这种情况一般非常低）。
    // 2. 为了统计准确请自行做好缓存，避免重复统计安装，只要 get 方法正常返回（【即：无异常】。无论 code 是否为空字符），以后都不应该再调用 Codeinstallsdk 的任何方法。
    // 3. 建议结合自身的用户账号系统，将获取到的 code（无论 code 是否为空字符） 和用户账号关联。比如：等用户用账号登录后决定是否获取 code，如果当前账号曾经获取过 code （无论曾经 code 是否为空字符），将不再获取。

    try{
        Codeinstallsdk.init();
        String code = Codeinstallsdk.get("CodeInstall提供给您的APPID", Build.VERSION.RELEASE, Build.MODEL);
    } catch (Exception e) {
        // 处理异常， 比如网络错误
    }

    ```

## iOS 端

1. 下载 SDK：[Codeinstallsdk.xcframework](Codeinstallsdk.xcframework)

2. 事例

    ```
    import Codeinstallsdk
    import UIKit

    ...

    // 【重要】
    // 1. 调用 get 方法，如果 code 不为空字符串，我们即可能（会尽量去重）认为是【一次有效匹配安装】；如果 code 为空字符串，我们即认为是【一次有效未匹配安装】（这种情况一般非常低）。
    // 2. 为了统计准确请自行做好缓存，避免重复统计安装，只要 get 方法正常返回（【即：无异常】。无论 code 是否为空字符），以后都不应该再调用 Codeinstallsdk 的任何方法。
    // 3. 建议结合自身的用户账号系统，将获取到的 code（无论 code 是否为空字符） 和用户账号关联。比如：等用户用账号登录后再决定是否获取 code，如果当前账号曾经获取过 code （无论曾经 code 是否为空字符），将不再获取。

    CodeinstallsdkInit()
    var err: NSError? = nil
    var code = CodeinstallsdkGet("CodeInstall提供给您的APPID", UIDevice.current.systemVersion, "iPhone", &err)
    if err != nil {
        // 处理异常， 比如网络错误
    }
    ```
