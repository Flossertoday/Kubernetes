## Referencing Guidance

https://www.cnblogs.com/2021-sue-888/p/14767120.html

## Error Msg

```
Errors during downloading metadata for repository 'kubernetes':
  - Status code: 404 for https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el8-x86_64/repodata/repomd.xml (IP: 119.96.66.248)
Error: Failed to download metadata for repo 'kubernetes': Cannot download repomd.xml: Cannot download repodata/repomd.xml: All mirrors were tried
```

## Analysis

Actually, the problem came from the source itself. It seems that mirrors.aliyun.com only provides up to el7 of kubernetes instead of el8 as indicated from the source. 

    vi /etc/yum.repos.d/kubernetes.repo

Then change key **baseurl**. 

