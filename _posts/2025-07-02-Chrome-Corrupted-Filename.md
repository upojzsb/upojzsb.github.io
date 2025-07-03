---
layout:     post
title:      Chrome Corrupted Filename
subtitle:   Chrome下载文件名乱码
date:       2025-07-02
author:     UPOJZSB
header-img: img/post-bg-random.jpg
catalog: true
tags:
    - Chrome
    - Encoding
---

# Prelogue

## English

Due to the encoding behavior built into Chrome (and other modern browsers such as Edge and Firefox), filenames containing Chinese characters may become corrupted when downloading files. For example, a file intended to be named `D01 首页.doc` might be saved as `D1Ê×Ò³.doc`.

According to ChatGPT's explanation:
> The server sends the Content-Disposition header using a legacy or incorrect encoding (typically GBK or other locale-based encoding), but Chrome (and all modern browsers) strictly interpret this header using UTF-8. As a result, non-ASCII characters are misinterpreted, producing messy or unreadable filenames.

Since we cannot modify the server's encoding behavior, the most practical solution is to fix the filenames locally after download. The following Python script can be used to automatically repair the corrupted filenames.

# Chinese

由于 Chrome 浏览器（以及其他现代浏览器如 Edge、Firefox）内置的字符编码策略，当下载包含中文字符的文件名时，文件名往往会出现乱码。例如，原本应为 `D01 首页.doc` 的文件，可能会被保存为 `D1Ê×Ò³.doc`。

根据 ChatGPT 的解释：
> 服务器在返回响应时，使用了传统或不标准的字符编码（通常是 GBK）设置 `Content-Disposition` 响应头中的文件名字段，而 Chrome（以及所有现代浏览器）会严格按 UTF-8 解读该字段。因此，非 ASCII 字符就会被误解码，导致乱码。

由于我们无法更改服务器端的编码行为，最实用的解决方法是：**在文件下载完成后，手动在本地修复文件名**。下面的 Python 脚本可以自动修复这些被误解码的文件名。


# Code

```python
import os

def fix_filename_encoding(folder):
    for name in os.listdir(folder):
        try:
            new_name = name.encode('latin1').decode('gbk')
            if new_name != name:
                old_path = os.path.join(folder, name)
                new_path = os.path.join(folder, new_name)
                os.rename(old_path, new_path)
                print(f'✅ {name} → {new_name}')
        except Exception:
            continue

if __name__ == '__main__':
    import sys
    if len(sys.argv) >= 2:
        target_folder = sys.argv[1]
    else:
        target_folder = '.'
    fix_filename_encoding(target_folder)
    print("\n✅ 修复完成，按回车键退出。")
    input()
```