---
title: '[Win10 pro]在Windows 10專業版中快速啟用/禁用工作管理員(Task Manager)'
date: 2022-07-18 19:38:07
categories: Win10
tags: [Windows, Win10 pro, Windows專業版, 工作管理員, Task Manager, taskmgr, bat, Batchfile,coding, code]
---
在Windows 10專業版中工作管理員被禁用該怎麼辦?  
其實Google給很多答案，包含編輯群組原則、改機碼/登錄編輯程式(regedit)等  
這些方法的麻煩點就是要在各個視窗游移，點來點去，忘了就要再Google  
那如果有bat檔下指令就快多了  
這裡就寫了一個  
<!--more-->  
有需要可以直接到[這裡](https://github.com/leeyaowen/Task-Manager-switch)下載  
記得以系統管理員身分執行
按Y可以啟用，按N(或其他按鍵)可以禁用  
code如下  
{% codeblock Batchfile lang:Batchfile %}
@echo off
set /p var=Turn on Taskmgr?[Y/N]: 
if %var%== Y reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System /v DisableTaskMgr /t REG_DWORD /d 0 /f

rem disable Task Manager
if not %var%== Y reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System /v DisableTaskMgr /t REG_DWORD /d 1 /f


pause
{% endcodeblock %}  