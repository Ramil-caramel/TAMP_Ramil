##Laboratory work VIII
Данная лабораторная работа посвещена изучению систем автоматизации развёртывания и управления приложениями на примере Docker
```
$ export GITHUB_USERNAME=<имя_пользователя>
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
$ git clone https://github.com/${GITHUB_USERNAME}/lab07 lab08
$ cd lab08
$ git submodule update --init
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08
```
делаем то же самое что и в предыдущих лабортаорных
Заиписываем конфигурацию нашего докер файла
```bash
$ docker build -t logger .
```
запускаем сборку докера
```
=> [internal] load build definition from Dockerfile                                  0.0s
 => => transferring dockerfile: 757B                                                  0.0s
 => [internal] load metadata for docker.io/library/ubuntu:18.04                       0.5s
 => [internal] load .dockerignore                                                     0.0s
 => => transferring context: 2B                                                       0.0s
 => CACHED [1/9] FROM docker.io/library/ubuntu:18.04@sha256:152dc042452c496007f07ca9  0.0s
 => [internal] load build context                                                     0.1s
 => => transferring context: 63.60kB                                                  0.0s
 => [2/9] RUN apt update && apt install -yy gcc g++ cmake curl                       31.6s
 => [3/9] RUN curl -LO https://github.com/Kitware/CMake/releases/download/v3.22.2/cm  8.8s 
 => [4/9] COPY . /print/                                                              0.4s 
 => [5/9] WORKDIR /print                                                              0.1s 
 => [6/9] RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=  37.9s 
 => [7/9] RUN cmake --build _build                                                    0.8s 
 => [8/9] RUN cmake --build _build --target install                                   0.4s 
 => [9/9] WORKDIR _install/bin                                                        0.1s 
 => exporting to image                                                                1.1s 
 => => exporting layers                                                               1.0s 
 => => writing image sha256:28cbee50c5d7a3580099fce26a80812250656769df4f708ed1286165  0.0s 
 => => naming to docker.io/library/logger                                             0.0s

```
```bash
docker images
```
```
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
logger       latest    28cbee50c5d7   18 seconds ago   484MB
```

```bash
$ mkdir logs
$ docker run -it -v "$(pwd)/logs/:/home/logs/" logger
text1
text2
text3
<C-D>
$ docker inspect logger
$ cat logs/log.txt
```
Монтируем контейнер, вводим текст, просим информацию о нем и выводим логи
```
{
        "Id": "sha256:28cbee50c5d7a3580099fce26a80812250656769df4f708ed1286165d291b5f8",
        "RepoTags": [
            "logger:latest"
        ],
        "RepoDigests": [],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2025-03-22T20:25:41.872703488+03:00",
        "DockerVersion": "",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LOG_PATH=/home/logs/log.txt"
            ],
            "Cmd": null,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "/print/_install/bin",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "./demo"
            ],
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "18.04"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 483851509,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/snap/docker/common/var-lib-docker/overlay2/runzp5tea52zcju1jh7t4izd3/diff:/var/snap/docker/common/var-lib-docker/overlay2/ioha9c6vrcrouionnq2a5emed/diff:/var/snap/docker/common/var-lib-docker/overlay2/vjq1s93e5dcmw4v9jhtla5z3l/diff:/var/snap/docker/common/var-lib-docker/overlay2/nc7tixcz1nyw858518cp4yuvw/diff:/var/snap/docker/common/var-lib-docker/overlay2/il2tuhe36hgddwuy73y32bov3/diff:/var/snap/docker/common/var-lib-docker/overlay2/yhw1fdv3e5mtgvhbswn5fn2t6/diff:/var/snap/docker/common/var-lib-docker/overlay2/17d3rd0plll1nm1t1yf0jnml4/diff:/var/snap/docker/common/var-lib-docker/overlay2/e65c25323b3c8ae2328b760d66004b2ae7a964cc0352cdd69299d3024604b4a5/diff",
                "MergedDir": "/var/snap/docker/common/var-lib-docker/overlay2/bka9d5fv9zcy9wo036tfa9srt/merged",
                "UpperDir": "/var/snap/docker/common/var-lib-docker/overlay2/bka9d5fv9zcy9wo036tfa9srt/diff",
                "WorkDir": "/var/snap/docker/common/var-lib-docker/overlay2/bka9d5fv9zcy9wo036tfa9srt/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:548a79621a426b4eb077c926eabac5a8620c454fb230640253e1b44dc7dd7562",
                "sha256:efa3d41a2527ce19e0ff9d45059db746ce010a12ac3fc32db74aed12cfe64a50",
                "sha256:174485565a2acf0fb07025b3e513bd3d5c157fe0771a51dc582ccec1c389b687",
                "sha256:60535b549fffed86dd9e8602e8051d1712456624ae00ad8e8d100d38122dbdf9",
                "sha256:5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef",
                "sha256:f85d19adfa031df5d7c5513f14430fcc706880328e96fddc0bff2d6955766f4e",
                "sha256:5b993e327d6e15ce4a2dcf3238bcb1502ae4c52bedb252c93e5c17dd5764cb11",
                "sha256:c019ca31cf92914537a9dbb683193e88092d37fe938650f1fd2f35da9e31ead9",
                "sha256:5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef"
            ]
        },
        "Metadata": {
            "LastTagTime": "2025-03-22T20:25:42.926379178+03:00"
        }
    }


text1
text2
text3

```
Далее комитим и отправляем на удаленный ркпозиторий
