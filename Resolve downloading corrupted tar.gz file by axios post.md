# Resolve downloading corrupted tar.gz file by axios post

#### Backend code is like:
```
    header('Content-Description: File Transfer');
    header('Content-Type: application/x-tar');
    header('Content-Disposition: attachment; filename='.basename($file));
    header('Content-Transfer-Encoding: binary');
    header('Expires: 0');
    header('Cache-Control: must-revalidate');
    header('Pragma: public');
    header('Content-Length: ' . filesize($file));
    flush();
    readfile($file);
    exit;
```
reference: 
[https://stackoverflow.com/questions/11315951/using-the-browser-prompt-to-download-a-file](https://stackoverflow.com/questions/11315951/using-the-browser-prompt-to-download-a-file)
#### And react code is like:
```
const params = new URLSearchParams();
    params.append('op', 'export_file');
    axios
      .post('url...', params)
      .then((res) => {
        const url = window.URL.createObjectURL(new Blob([res.data]));
        const link = document.createElement('a');
        link.href = url;
        link.setAttribute('download', this.state.export_file_name);
        document.body.appendChild(link);
        link.click();
        link.parentNode.removeChild(link);
      })
      .catch((error) => {});
```
and we get corrupted tar.gz file...

### Solution
1. check backend code, including original file which can be open.
2. compare with two files size by **chrome F12**.(original file vs downloaded file)
3. check frontend code.

The problem is that I didn't set responseType parameter to axios.
reference:
[https://gist.github.com/javilobo8/097c30a233786be52070986d8cdb1743#file-download-file-js-L6](https://gist.github.com/javilobo8/097c30a233786be52070986d8cdb1743#file-download-file-js-L6)

some resources about responseType:
* [Mozilla responseType](https://developer.mozilla.org/zh-TW/docs/Web/API/XMLHttpRequest/responseType)
* [Mozilla Blob](https://developer.mozilla.org/zh-TW/docs/Web/API/Blob)
* [MOzilla ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)
* [ArrayBuffer vs Blob](https://github.com/abbshr/abbshr.github.io/issues/28)


