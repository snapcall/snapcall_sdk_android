# Snapcall Android SDK

v2.8.2

## Documentation

see [Snapcall documentation](https://doc.snapcall.io/#android)

## Integration gitHub Release

we provide the aar as a github release, the script provided here download the requested version and install it to a libs folder
```gradle
def getGitHubRelease = { name, githubRepo = "snapcall/snapcall_sdk_android" ->
    Collection v = name.split(":")
    if (v.size() != 2)
        throw new GradleException("getGithubRelease: wrong aar name [name:release]")
    String aarVersion = v.get(1)
    String aarName = v.get(0)
    String url = "https://github.com/${githubRepo}/releases/download/"
    File file = new File("$projectDir/libs/${name}.aar")
    if (!file.exists()) {
        delete fileTree("$projectDir/libs/").matching { include "${aarName}*.aar" }
        file.parentFile.mkdirs()
        try {
            new URL("${url}${aarVersion}/${aarName}.aar").withInputStream { downloadStream ->
                file.withOutputStream { fileOut ->
                    fileOut << downloadStream
                }
            }
        } catch (Exception e) {
            throw new GradleException("getGithubRelease: could not download aar file")
        }
    }
    if (!file.exists())
        throw new GradleException("getGithubRelease: could not find aar file")
    files(file.absolutePath)
}

```

```
implementation getGitHubRelease('snapcall_android_framework:2.8.2')
implementation 'com.squareup.okhttp3:okhttp:4.2.0'
implementation 'org.webrtc:google-webrtc:1.0.32006'
``` 
