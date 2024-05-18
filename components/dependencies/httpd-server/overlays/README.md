# httpd container used for iso


**Manually setup deployment**
```
oc apply -k httpd-server/overlays/default
```

**Windows Server 2019 Download location**
https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019

**Download Example**
```
URL_LOCATION="https://go.microsoft.com/fwlink/p/?LinkID=xxXxXxXx&clcid=0x409&culture=en-us&country=US"
curl -L "$URL_LOCATION" -o win2k19.iso
```
**Copy ISO to httpd container**
```
ISO_FILE=$HOME/win2k19.iso
POD_NAME=$(oc get pods --selector=app=httpd-server -o jsonpath='{.items[0].metadata.name}' -n httpd-server)
oc cp $HOME/win2k19.iso $POD_NAME:/opt/app-root/src -n httpd-server
```
