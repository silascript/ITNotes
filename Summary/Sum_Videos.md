---
aliases:
  - 
tags:
  - video
  - summary
created: 2023-06-28 5:47:54
modified: 2023-06-28 6:03:54
---
# 视频汇总

---

## 视频笔记

```dataview
Table WITHOUT ID
file.link as "文件名称",dateformat(file.cday,"yyyy-MM-dd") as "创建时间",
dateformat(file.mday,"yyyy-MM-dd") as "修改时间"
where icontains(file.name,"Video") and file.name != "Sum_Videos"
```

