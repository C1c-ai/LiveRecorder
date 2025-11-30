📘 LiveRecorderGUI 使用说明

1. 文件结构
----------------
解压后目录大致如下：
LiveRecorderGUI/
 ├─ LiveRecorderGUI.exe            # 主程序，双击启动
 ├─ live_recorder_config.json      # 配置文件（可选）
 ├─ bin/
 │   ├─ yt-dlp.exe                  # 内置下载器
 │   └─ ffmpeg.exe                  # 内置转码工具
 └─ recordings/                     # 录制文件输出目录（程序运行时会生成）

2. 快速开始
----------------
1. 解压 zip 到任意文件夹（不要放在中文路径或有空格的路径，避免兼容问题）
2. 双击 LiveRecorderGUI.exe 打开软件
3. 在软件里：
   - 点击 添加，填写直播间名称和 URL
   - 选择清晰度（推荐填 best，自动最高）
   - 容器选择 mp4
   - 等待时间 365d 表示最多等待一年（你可以改成 1h, 12h 等）
4. 点击 开始 或 全部开始，等待开播 → 自动录制

录制文件默认会保存在 recordings/ 文件夹里。

3. 配置文件说明
----------------
程序会在同目录下读取/保存 live_recorder_config.json。
配置文件包含全局设置和房间列表（比如 URL、清晰度等）。
你可以手动修改，也可以在 GUI 里操作 → 关闭时会自动保存。

示例配置：
{
  "global_out_dir": "recordings",
  "no_part": true,
  "threads": 8,
  "rooms": [
    {
      "name": "B站直播1",
      "url": "https://live.bilibili.com/650",  这里要输入简洁房间号例如 （https://live.bilibili.com/146007）
      "quality": "best",
      "container": "mp4",
      "wait_for": "365d",
      "output_dir": "",
      "proxy": ""
    }
  ]
}

4. 常见问题
----------------
- 程序打不开？
  请确认已完整解压 zip（不要直接在压缩包里运行）。
- 录制不了？
  检查 URL 是否正确，B站直播需要用 https://live.bilibili.com/房间号
- 录制中断？
  软件会自动重试；如果还是失败，可能是网络问题，可以设置代理。
- 输出文件在哪？
  默认在 recordings/ 文件夹（可在界面修改）。

5. 建议
----------------
- 保持软件运行 → 它会一直等待开播并自动录制
- 建议每个房间单独一个配置，方便管理
- 如需换电脑，只要带上 LiveRecorderGUI.exe + bin + live_recorder_config.json 就能直接用

6.常见问题
----------------
关闭软件时WARNING：failed to remove temporary directory 
----------------
这个提示其实是 PyInstaller 的 onefile 模式常见小问题，不影响正常使用。
它的意思是：EXE 解压到临时目录（_MEIxxxxx），退出时没能删除干净。

原因一般有两个：
	1.	杀毒/安全软件拦截：Windows 有时会拦截 PyInstaller 自清理
	2.	文件句柄未释放：比如 ffmpeg 或 yt-dlp 进程没完全退出，导致临时目录被占用