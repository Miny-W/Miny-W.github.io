---
title: "C#通过命令行调用python脚本"
categories: C#
tags: C#
author: Miny
---

## 前提

忽然接到一个甲方需求，要我从WebGL获取Python导出的一套图片。本文将说明该流程中是如何利用C#调用.py的。

## 正文

```c#
 string InvokePython(string str)
        {
            string CmdPath = @"C:\Windows\System32\cmd.exe";//Cmd路径
            ProcessStartInfo processInfo = new ProcessStartInfo();
            processInfo.FileName = CmdPath;
            //   Process.Start(processInfo);
            using (Process proc = new Process())
            {
                string cmd1 = @"cd C:\Users\Admin\Desktop\iFlow-master\iFlow-master";//.py的文件夹路径
                string cmd2 = @"python main.py G:\Test_Python\" + str+" input/"+txtName+".txt";//启动python的命令以及传入参数
                //向cmd窗口写入命令
                proc.StartInfo.CreateNoWindow = true;//无cmd窗口
                proc.StartInfo.FileName = CmdPath;
                proc.StartInfo.UseShellExecute = false;
                proc.StartInfo.RedirectStandardError = true;
                proc.StartInfo.RedirectStandardInput = true;
                proc.StartInfo.RedirectStandardOutput = true;
                proc.Start();
                proc.StandardInput.WriteLine(cmd1);
                proc.StandardInput.WriteLine(cmd2);
                proc.StandardInput.WriteLine("exit");
                string outStr = proc.StandardOutput.ReadToEnd();
                proc.Close();
                Send("C_Finish_OpenPython");//服务器将python运行完毕的消息发给客户端
                Console.WriteLine("python运行完毕");//服务器打印
                return outStr;
            }
        }
```



## 结尾

可以根据实际用法，考虑是否增加一层Thread壳。

不只是.py，任何windows系统可运行的软件，均可用这套思路，通过cmd命令行启动。

