title: 清理 Macbook 磁盘空间
date: 2018-12-21 21:44:23
updated: 2018-12-22 21:52:15
tags:
---

# 清理 Macbook 磁盘空间

因为当初买的是丐版 Macbook，因此经常面临磁盘空间告警的情况，所以

## 查看目录大小

推荐使用 OmniDiskSweeper，`brew cask install omnidisksweeper`。打开后点击当前磁盘，进入后等待一段时间就能看到各个目录的占用空间大小，也可以逐一点进去看到子目录的大小。

## Homebrew 的清理

平时更新 Homebrew 时，及时进行 cleanup 操作，或者直接使用命令 `brew update && brew upgrade && brew cleanup`，这样就能够及时清理掉旧的软件版本。

此外，随着使用时间的增长，Homebrew 的 git 文件夹也会变的越来越大，因此可以通过 `git gc` 指令来整理 git 文件夹。这个原理是平时每次更新 git 都会下载各种更新文件，而这些文件会在磁盘保存为一种称作松散对象 (loose object) 格式，通过运行 `gc` 指令，git 会将这些平时零散存储的文件对象打包至一个叫 packfile 的二进制文件以节省空间并提高效率。如果时间长了，则有可能缩减几十万个文件。

![清理 Homebrew 的效果](/img/cleanmac.png)

为了能够自动清理，我编写了如下的脚本，以后定期执行即可。

```
#!/bin/sh

cd "$(brew --repo)" && git gc --prune=now

root="$(brew --repo)/Library/Taps/homebrew/"

echo $root
for d in `ls $root`
do
    echo " clean $root$d ..."
    cd $root$d && git gc --prune=now
done

root="$(brew --repo)/Library/Taps/caskroom/"
echo $root
for d in `ls $root`
do
    echo " clean $root$d ..."
    cd $root$d && git gc --prune=now
done
```

## Xcode 清理

Xcode 会下载大量的文件，但是过了一段时间后，仍然会有旧版本设备的支持，所以需要清理如下文件夹：

```
~/Library/Developer/Xcode/DerivedData
~/Library/Developer/Xcode/Archives
~/Library/Developer/Xcode/iOS DeviceSupport
~/Library/Developer/Xcode/watchOS DeviceSupport
~/Library/Developer/CoreSimulator 
```

## MongoDB 清理

长久未使用 MongoDB，发现随着版本升级，对应的数据库文件没有升级，因此已经无法正常使用了，而 journal 日志则占用了将近 1G 空间，因此不得不先对数据库文件进行升级。通过 MongoDB 的启动日志可以知道，需要先从 3.4 升级到 3.6 再升级到 4.0。具体的脚本内容如下：

```
brew install mongodb@3.4
brew services start mongodb@3.4
/usr/local/opt/mongodb@3.4/bin/mongo
> db.adminCommand( { setFeatureCompatibilityVersion: "3.4" } )
brew uninstall mongodb@3.4

// 安装 MongoDB 3.6
brew services start mongodb@3.6
/usr/local/opt/mongodb@3.6/bin/mongo
> db.adminCommand( { setFeatureCompatibilityVersion: "3.6" } )
brew services stop mongodb@3.6
brew uninstall mongodb@3.6

// 安装 MongoDB 4
brew install mongodb
brew services start mongodb
```

升级完毕后，发现存储引擎仍然为 MMAPV1，需要升级为 wiredTiger，因此我在导出数据库后，删除数据库文件夹后又重装了 MongoDB 再导入数据，数据库占用空间也瞬时缩减了一大半。

```
mongodump --out mongodb
brew uninstall mongodb
rm -rf /usr/local/var/mongodb
brew install mongodb
brew services start mongodb
mongorestore mongodb
```

## 引用
[为什么你的 Git 仓库变得如此臃肿](https://www.jianshu.com/p/7231b509c279)
[Can I delete data from iOS DeviceSupport?](https://stackoverflow.com/questions/29930198/can-i-delete-data-from-ios-devicesupport/29931912)