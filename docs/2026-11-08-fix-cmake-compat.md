# 修复记录：CI CMake 版本兼容性失败（2026-11-08）

## 失败信息
- 仓库：wiseism/Router-Lab
- Workflow：C/C++ CI
- 失败 run：[42426838464](https://github.com/wiseism/Router-Lab/actions/runs/42426838464)（2026-11-07）、[42013498859](https://github.com/wiseism/Router-Lab/actions/runs/42013498859)（2026-11-05）
- 失败平台：ubuntu-latest / macos-latest（Configure 阶段）

## 错误日志摘录
```
CMake Error at Example/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 has been removed from CMake.
  Update the VERSION argument <min> value.
-- Configuring incomplete, errors occurred!
##[error]Process completed with exit code 1.
```

## 原因分析
`Example/CMakeLists.txt` 声明 `cmake_minimum_required(VERSION 3.0)`。
runner 环境的 CMake 已移除对 < 3.5 版本策略的兼容支持，Configure 阶段直接报错，
ubuntu job 因此被取消。

## 修复内容
- `Example/CMakeLists.txt`：`cmake_minimum_required(VERSION 3.0)` →
  `cmake_minimum_required(VERSION 3.10)`，与根目录 `CMakeLists.txt` 保持一致。

## 验证
| run id | 结果 |
| --- | --- |
| 42440925102 | ✅ success |
