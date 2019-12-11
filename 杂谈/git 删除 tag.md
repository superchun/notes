首先删除本地 tag

```sh
git tag -d tag_name
```

删除本地 tag 后，删除服务器 tag

```shell
git push origin :refs/tags/tag_name
```

