### HTTP interface
+ [`/DCIM/`](#iface_list_directory)
+ [`/getconfig`](#iface_getconfig)
+ [`/setconfig`](#iface_setconfig)
+ [`/af`](#iface_af)
+ [`/to_videomode`](#iface_to_photomode)
+ [`/to_photomode`](#iface_to_photomode)
+ [`/info`](#iface_info)
+ [`/capture`](#iface_capture)
+ [`/nk_nonaf_capture`](#iface_nk_nonaf_capture)
+ [`/preview`](#iface_preview)
+ [`/bulb`](#iface_bulb)
+ [`/contcap`](#iface_contcap)
+ [`/nk_ctrl_mode`](#iface_nk_ctrl_mode)

####/DCIM/
<a name="iface_list_directory"> </a>

```
Function:
  1, List folders    eg. /DCIM/
  2, List files      eg. /DCIM/100MEDIA/
  3, Get file        eg. /DCIM/100MEDIA/DSC_0001.JPG

Supported Params:
  quick=1
  image=1     // list image
  video=1     // list video
  raw=1       // list raw
  start=1     // start from index=1
  limit=100   // list only 100 file
  thumb=1     // return thumbnail
  slot=0      // when camera have two cards. use slot=1 to the other card slot

Return Value Format:
  IMAGE + "/n" + "IMAGE" + "/n" + ...

Example:
> http://10.98.32.1:8080/DCIM/
102D7100,6,DSC_7754.JPG,DSC_7753.JPG,DSC_7752.JPG

> http://10.98.32.1:8080/DCIM/?quick=1
102D7100

> http://10.98.32.1:8080/DCIM/102D7100/?quick=1
DSC_7514.MOV
DSC_7512.MOV
DSC_7511.MOV
DSC_7509.MOV
DSC_7755.JPG
DSC_7754.JPG

> http://10.98.32.1:8080/DCIM/102D7100/?quick=1&image=1
DSC_7755.JPG
DSC_7754.JPG
DSC_7753.JPG

> http://10.98.32.1:8080/DCIM/102D7100/?quick=1&video=1
DSC_7614.MOV
DSC_7613.MOV
DSC_7612.MOV

> http://10.98.32.1:8080/DCIM/102D7100/?quick=1&video=1&image=1
DSC_7514.MOV
DSC_7512.MOV
DSC_7755.JPG
DSC_7754.JPG
DSC_7753.JPG

> http://10.98.32.1:8080/DCIM/102D7100/?raw=1 // return only RAW file
DSC_7691.NEF
DSC_7690.NEF

> http://10.98.32.1:8080/DCIM/102D7100/?raw=1&start=1&limit=3   // return 3 RAW files started from the 2nd file.
DSC_7690.NEF
DSC_7689.NEF
DSC_7688.NEF

```

### /getconfig
<a name="iface_getconfig"> </a>

```
Function:
  get one config of camera

Supported Params:
  name={exposurecompensation,shutterspeed,aperture,iso,whitebalance}

Example:
> http://10.98.32.1:8080/getconfig?name=exposurecompensation
5|0|Exposure Compensation|exposurecompensation:4|-5,-4.6,-4.3,-4,-3.6,-3.3,-3,-2.6,-2.3,-2,-1.6,-1.3,-1.0,-0.6,-0.3,0,0.3,0.6,1.0,1.3,1.6,2,2.3,2.6,3,3.3,3.6,4,4.3,4.6,5

> http://10.98.32.1:8080/getconfig?name=shutterspeed
5|0|Shutter Speed|shutterspeed:1/15|1/8000,1/6400,1/5000,1/4000,1/3200,1/2500,1/2000,1/1600,1/1250,1/1000,1/800,1/640,1/500,1/400,1/320,1/250,1/200,1/160,1/125,1/100,1/80,1/60,1/50,1/40,1/30,1/25,1/20,1/15,1/13,1/10,1/8,1/6,1/5,1/4,1/3,10/25,1/2,10/16,10/13,1,13/10,16/10,2,25/10,3,4,5,6,8,10,13,15,20,25,30,65535/65533,65535/65534,65535/65535
```

### /setconfig
<a name="iface_setconfig"> </a>

```
Function:
  update camera status

Supported Params:
  config_name=value   // config_name is just like those in getconfig

Return Value:
  OK
  FAILED

Example:
>http://10.98.32.1:8080/setconfig?shutterspeed=1/2
OK
>http://10.98.32.1:8080/setconfig?shutterspeed=aaa
FAILED

>http://10.98.32.1:8080/setconfig?shutterspeed=1
OK

```

### /af
<a name="iface_af"> </a>

```
Function:
  do auto-focus

Supported Params:
  position=1000,2000   // do af at posotion(1000,2000)

Return Value:
  OK
  FAILED

Example:
>http://10.98.32.1:8080/af                    // do auto-focus
OK
>http://10.98.32.1:8080/af?position=100,200   // set the focus point
OK

```

### /to_videomode
### /to_photomode
<a name="iface_to_photomode"> </a>

```
Function:
  switch to video/photo mode

Supported Params:
  nil

Return Value:
  OK
  FAILED

Example:
>http://10.98.32.1:8080/to_videomode
OK
>http://10.98.32.1:8080/to_photomode
OK
```

### /info
<a name="iface_info"> </a>

```
Function:
  1, get weyefeye firmware type and version
  2, get current connected camera
  3, get weyefeye status (idle,busy,capturing,liveviewing..)

Supported Params:
  nil

Example:
>http://10.98.32.1:8080/info
>0.10.8-6baa960|4b0|42a|Nikon Corporation|D800|V1.02|0|wf2|90466100000000000000000000000000
```

### /capture
<a name="iface_capture"> </a>

```
Function:
  capture an image

Supported Params:
  af=0
  af=1    // do auto-focus before capture

Return value:
  OK,"IMAGE_PATH"
  FAILED

Example:
>http://10.98.32.1:8080/capture?af=1
OK,/slot0/DCIM/116ND800/DSC_3018.JPG

>http://10.98.32.1:8080/capture?af=1
FAILED
```

### /nk_nonaf_capture
<a name="iface_nk_nonaf_capture"> </a>

```
Function:
  capture an image without auto-focus (special workaround for some of Nikon Camera)

Supported Params:
  nil

Return value:
  OK,"IMAGE_PATH"
  FAILED

Example:
>http://10.98.32.1:8080/nk_nonaf_capture
OK,/slot0/DCIM/116ND800/DSC_3019.JPG

>http://10.98.32.1:8080/nk_nonaf_capture
FAILED
```
