## Windows 中使用 wrk 进行压测

拉取 docker 镜像

```bash
docker pull williamyeh/wrk
```

编写 Lua 脚本

```lua
wrk.method = "POST"
wrk.headers["Content-Type"] = "application/json"
wrk.body = '{"name": "Lim"}'
```

运行压测命令

```bash
docker run -it --rm -v "%cd%":/data williamyeh/wrk -t1 -c400 -d10s -s test.lua http://172.18.61.177:3000/api/user
```

- docker 参数解释
  - `--rm`：docker 容器退出后直接删除对应容器
  - `-v`：`"%cd%"` 在 windows 环境中代表当前文件夹路径
- wrk 参数解释
  - `-t`：启动线程数
  - `-c`：启动连接数
  - `-d`：压测持续时间
  - `-s`：使用的 Lua 脚本