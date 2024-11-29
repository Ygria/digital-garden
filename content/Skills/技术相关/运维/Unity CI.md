## 参考：

1、[https://game.ci/docs/](https://game.ci/docs/) 
https://hub.docker.com/r/unityci/editor
用于提供镜像化的Unity编译环境

2、参考 [https://docs.unity3d.com/cn/2021.1/Manual/ManualActivationGuide.html](https://docs.unity3d.com/cn/2021.1/Manual/ManualActivationGuide.html) + [https://www.lotlab.org/2023/12/03/install-unity/](https://www.lotlab.org/2023/12/03/install-unity/) ，进行编译环境的证书激活。（由于Unity中国禁止绕过Unity Hub的个人版的手动和命令行激活，故需注册外网账号 + 去外网激活。）

3、[https://www.lotlab.org/2020/08/29/re-build-unity-project-with-jenkins-on-docker/](https://www.lotlab.org/2020/08/29/re-build-unity-project-with-jenkins-on-docker/) Jenkins Pipeline脚本编写

4、[https://gitlab.com/game-ci/unity3d-gitlab-ci-example](https://gitlab.com/game-ci/unity3d-gitlab-ci-example) 进行实际打包命令 + csharp工程编译入口编写

## 步骤：

1、在需要激活的machine（有可能是容器内）生成Manual许可.alf文件

2、进入Unity官网，Manual License激活，需要F12将隐藏的元素修改为展示，上传alf文件并手动激活后，下载License文件（.ulf）

3、将凭据上传到Jenkins中，在构建时将文件放在容器内/root/.local/share/unity3d/Unity/中

4、待编译的工程中，需加入编译入口脚本（ csharp），可直接使用： [https://gitlab.com/game-ci/unity3d-gitlab-ci-example/-/blob/main/Assets/Scripts/Editor/BuildCommand.cs](https://gitlab.com/game-ci/unity3d-gitlab-ci-example/-/blob/main/Assets/Scripts/Editor/BuildCommand.cs)

3、使用GameCI Docker镜像进行构建。
