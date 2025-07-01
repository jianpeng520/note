## 1.查看datax是否安装：linux中输入
`cd /opt/software/datax/` 

`find /opt/software/datax/ -type f -name "._*er" | xargs rm -rf` 

`python2 bin/datax.py job/job.json`

## 2.获取官方模版，体验一把datax
### a.linux中，输入以下命令，获取json模版
`python2 /opt/software/datax/bin/datax.py -r streamreader -w streamwriter` 
### b.在linux中编辑   vim  /lx/j1.json
{
    "job": {
        "content": [
            {
                "reader": {
                    "name": "streamreader", 
                    "parameter": {
                        "column": [
							{"type":"string", "value":"我想起那天夕阳下的奔跑"},
							{"type":"string", "value":"那是我逝去的青春"}
						], 
                        "sliceRecordCount": "5"
                    }
                }, 
                "writer": {
                    "name": "streamwriter", 
                    "parameter": {
                        "encoding": "", 
                        "print": true
                    }
                }
            }
        ], 
        "setting": {
            "speed": {
                "channel": "1"
            }
        }
    }
}

## c.linux中，运行json脚本
`python2  /opt/software/datax/bin/datax.py   /lx/j1.json`