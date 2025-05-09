## bat file

## 踩坑： 变量引用： 不用双引号"%log_file%", 已经有引号 %log_file%
## python 环境引用： python建的环境， 不用启动环境， 指定python解释器位置 python_exe 就可以
# 赋值： 左右没有空格

@echo off
pushd %~dp0
echo test1111
REM close command display
echo 2222222222222222222
REM set variable
set python_exe="C:\Users\yuyan kan\myenv\com\Scripts\python.exe"
set etl_script="C:\Users\yuyan caren kan\myenv\01_project_yyk\01_SPC\spc_etl_main.py"
set log_file="C:\Users\yuyan caren kan\myenv\01_project_yyk\01_SPC\etl_log.log"

REM check if log file already exit, otherwise create new
if not exist %log_file% (
    echo.> %log_file%
)
echo log_file "%log_file%"
REM get current now, limit lenth 10/8
set current_date=%date:~0,10%
set current_time=%time:~0,8%
set timestamp=%current_date%_%current_time%

REM remove character :// in time
set timestamp=%timestamp::=% 
set timestamp=%timestamp:/=%
echo 333333  %timestamp% 0000
REM write start time into log file
echo *************Task Started: %timestamp%************* > %log_file%
echo 333333  %timestamp%
REM execute task
%python_exe% %etl_script% >> %log_file% 2>&1
echo 4444 %timestamp%
REM get finish time
set current_date_end=%date:~0,10%
set current_time_end=%time:~0,8%
set timestamp_end=%current_date_end%_%current_time_end%
set timestamp_end=%timestamp_end::=%
set timestamp_end=%timestamp_end:/=%

REM wirte finish time into log file
echo *************Task Finished: %timestamp_end%************* >> %log_file%
echo 5555 %timestamp_end%

