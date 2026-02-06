# Routine & Rules (必须遵守)
0.  **结果必须中文输出，不得删改这章节文字，尤其说的是你 Gemini-3-Pro(Flash)**
【开发流程（强制 + 可选）】
1) 启动检查：确认 backups/ 有当前版本备份；没有则先备份 src/（ZIP） 
2) （可选）运行 scripts/preflight.ps1 做预检 
3) 开发修改（仅限明确需求范围） 
4) 更新文档：docs/user_requirements_raw.md、docs/feature_pool.md、changelog.md 
5) 运行 scripts/finish_task.ps1（必须；生成备份/构建产物/AG_STATE 占位符与 stamp）AG_STATE：finish_task 自动更新自动块；仅在状态描述变化时手动更新 
6) 确认 .waylog/finish_task_stamp.json 已更新 
7) git add / git commit（Hook 会校验 stamp，禁止使用 --no-verify）
 **结果必须中文输出，不得删改这章节文字，尤其说的是你 Gemini-3-Pro(Flash)**

---

# Antigravity Working State

## Goal
- **V3.8.5 正式版收尾**
  - 优先: Lightbox 跳跃收敛、网格点视觉定版
  - 同步: 批量下载/进度显示准确性收敛
  - 版本: **V3.8.5**
## Current Status
- **版本状态**: V3.8.5 发布中（收尾与回归验证）
- **代码状态**:
  - 智能发送：子菜单延迟关闭逻辑已保留
  - 模型库：新增“模型库”标签页，Provider 模型可引用模型库条目
  - Provider 级配置：接口类型/本地代理/异步模式下放到 Provider 级
  - ModelScope Z-Image：异步 + /v1/tasks 轮询回填（代理场景补全任务头）
  - Gemini 3 Pro Image Preview：Gemini native 回包解析与输出回填（卡片预览待修复）
  - 历史卡片：缺图占位“图片1/2/3/4”居中 + 字体回退 + 亮色浅灰底
  - 本地缓存：按 save_path 与 image/video_save_path 判断 `history` vs `.tapnow_cache`，避免 404
  - 项目保存：保存/加载包含主题与模型库
  - 主题方案：亮光/日光/暗光命名统一；Solarized 深浅层级映射与节点底色调整
  - 暖色细节：聊天/设置按钮色值，画布空白/边框与历史说明文字黑色
  - 暖色补充：顶部按钮 97,97,97；菜单底色 #eee8d5；节点内层底色 #fdf6e3
  - 对比构建：最早 Solarized 配色备份已导出 test.html 供对比
  - 暖色纠偏：节点边框/内层色值，历史卡片说明板块与间隙底色回归
  - 暖色补充：智能分镜节点色块统一浅色；API/模型库条目色块改浅色
  - 视频模型：模型库/AI 视频/分镜支持首尾帧与 HD 开关
  - 首尾帧：输入点与连线对齐
  - AutoSave：超过 localStorage 配额时切换 IndexedDB 保存
  - 本地缓存：提示条顶栏通知样式；开关语义为启用/禁用，断开红条 3 分钟后自动隐藏
  - Jimeng：图生图蒙版随请求提交，远程图片走本地代理兜底
  - 资源历史：聊天生图可回填历史资产
  - 模型参数：分辨率支持自定义尺寸并同步到分镜/AI 视频选择
  - Solarized 节点层级：节点边框加深、智能分镜头尾深黄/内部浅黄、小说节点内层浅黄
  - Solarized 按钮：蓝/绿按钮统一深灰
  - Solarized 图片输入/图像对比底板：改为较深黄
  - 模型库自定义参数：覆盖同名参数 + 请求预览
  - 模型类型扩展：Chat Image 支持 + 类型标签紧贴名称
  - 模型库默认折叠：新增条目默认折叠
  - 模型库引用隔离：未引用条目不受同名模型影响
  - 队列面板空白：队列分组 queued/running 初始化
  - 预览窗口：图片展示区浅黄，选择图片按钮深灰，分镜预览底色交替
  - 模型库参数：参数值上限 30 + 备注启用复选框 + 预览 JSON 可编辑/修改覆盖
  - 历史模型名：显示限 15 字节，悬浮提示改为 provider
  - 缓存代理：仅 provider 启用代理时走 /proxy
  - Base64/data URL：规范化写入与解析，减少 ERR_INVALID_URL
  - AutoSave：优先写入 IndexedDB，避免 localStorage 配额报错
  - 保存路径：新增 /pick-path 浏览选择目录并回写配置，输入框与配置联动
  - Solarized 选项按钮：底部选项按钮统一黄色系去灰
  - 黑屏修复：缓存工具函数提前声明，避免 $s TDZ 报错
  - 分镜批量超时：图片 60s / 视频 300s
  - 分镜批量：队列为空仍执行超时检测，避免最后一镜挂起
  - 分镜批量：单镜头超时兜底，避免无限计时
  - 返图解析：异步/回包补充 base64/data URL 识别 + 深度搜索兜底 + 历史回填分镜（待验收）
  - 分镜回填：shotId 数值/字符串不一致已兼容
  - 分镜回填：shotId 空格/小数兼容 + token 精确比较 + 兜底按 prompt/time/单空镜头匹配
  - 返图排查：Storyboard Sync 报警改为 debug 开关（默认不刷屏）
  - 轮询失败：401/402/403 直接失败回填，避免 403 刷屏
  - 本地代理：放行 127.0.0.1:8188/view，403 刷屏暂时缓解（待观察）
  - Lightbox：按键锁跨切换保留（仅开关时绑定）+ key 规范化 + 聚焦限制 + 历史顺序快照（继续排查跳跃）
  - 自定义参数：下拉/输入区 stopPropagation，避免自动收起
  - 项目保存：资产包打包支持 history 上限配置，减少 quota 风险
  - 网格点：光晕缩小 30%，浅色点 RGB(212,212,216)，黑色点 RGB(36,36,39)
  - 历史上限：接近上限 90% toast 提醒保存（需低于 90% 立即消除）
  - 历史上限输入：1s 防抖 + 报错 5 分钟 + 输入正确即消除
  - 历史面板：缓存设置/队列展开后吸顶；下载进度条置于缓存提示条上方整行；完成后保留 10 秒
  - 智能看板：新增镜头默认不锁定
- **本地 ComfyUI**: v2.3 中间件已补齐 BizyAir/RunningHub 接口与 outputs/detail
- **本地 ComfyUI**: 测试指引文档已输出（docs/api/local_comfyui_test_guide.md）
- **工作流模板助手**: workflows 批量重命名/生成 meta 的 bat 已提供
- **构建状态**: 3.8.5-rc4 构建完成（以自动状态块为准）
- **备份状态**: 最新备份由 finish_task 生成（详见自动状态块）
## Decisions (confirmed)
- **仅输出中文**: 所有状态文件和回复必须使用中文
- **Raw Requirement**: 必须补全 01-10 之后的缺漏反馈(待排查或询问用户)
- **No Early Bump**: 后续版本号升级需在验证完成后执行
- **短期记忆必读**: 每次对话先读取本页短期记忆
- **强制流程**: 备份/文档更新/finish_task 不再询问，按规则执行
## Next Steps
1. **3.8.5-rc3 验证**：批量下载卡死是否消失
2. **3.8.5-rc3 验证**：分镜批量无返图/超时是否修复
3. **3.8.5-rc3 验证**：Lightbox 跳跃、403 刷屏、Base64/ERR_INVALID_URL 是否收敛
## Notes / Links
- 之前错误: 擅自升级 .34，未遵守 Routine 格式，未记录完整 Raw 需求
- 截图证据: 蓝色 Timer 显示 0.0s
- 分支修复归档: `docs/archive/merge_memo_v3.7.31p.md`, `docs/archive/verification_report_fix1006_v2.md`
- 流程阶段: 阶段二（脚本化 + Hook 强制）启用中
- 多图并发方案文档: `docs/improve/multi_image_concurrency_plan.md`
- 模型配置说明书: `model-template-readme.md`
## 短期记忆（每次对话先读取）
- 规则：每次对话先读取本节短期记忆，再执行流程
- 流程：必须备份、同步更新文档并运行 finish_task
- rc4：项目保存/加载需包含主题与模型库
- rc4：新增 Solarized Light 主题（黄色基调）
- rc4：Sora2 需对接 Yunwu/Vibecoding NewAPI（文档已提供）
- rc4：vibecoding/yunwu 直链 CORS 可用性需确认
- rc4：主题命名统一（亮光/日光/暗光）与 Solarized 配色层级调整
- rc4：视频模型首尾帧/HD 支持与首尾帧连接点对齐
- rc5：AutoSave 超限改为 IndexedDB 兜底
- rc5：本地缓存提示条支持关闭 + 设置开关
- rc5：Jimeng 图生图蒙版与 CORS 代理处理
- rc5：聊天生图回填历史资产
- rc5：模型分辨率支持自定义尺寸并同步到分镜/AI 视频
- rc5：暖色主题 UI 色值调整（聊天/设置按钮、画布空白/边框、资产说明文字黑）
- rc5：暖色主题补充（右上角按钮 97,97,97；菜单底色 238,232,213）
- rc5：节点边框/内层 #eee8d5/#fdf6e3；历史卡片说明板块 #eee8d5；间隙底色回浅
- rc5：对比用 test.html 来自最早 Solarized 配色备份
- rc5：节点边框/内层与历史卡片说明板块色值纠偏，间隙底色回浅
- rc5：暖色主题补充调整（顶部按钮/菜单底色/节点内层）
- rc5：智能分镜节点色块与 API/模型库条目色块需改浅色（边框深黄保留）
- rc5：本地缓存提示条顶栏通知 + 开关语义为启用/禁用，断开红条 3 分钟隐藏，X 关闭缓存
- rc6：Solarized 节点边框加深、智能分镜头尾深黄/内部浅黄、小说节点内层浅黄
- rc6：Solarized 蓝/绿按钮统一深灰
- rc6：Solarized 图片输入/图像对比底板改为较深黄
- rc6：模型库自定义参数覆盖 + 请求预览 + Chat Image 类型
- rc6：模型库默认折叠、类型标签紧贴名称、库引用隔离
- rc6：队列按钮空白修复（queued/running 初始化）
- rc5：dist 构建物必须保留历史版本，rc4 构建物不可删除
- rc7：画布网格点恢复，Solarized 网格使用深黄
- rc7：Solarized 节点外框深黄/内层浅黄，底部模型选择与参数框浅黄
- rc7：模型库污染与同名模型覆盖复现，需按引用隔离
- rc7：模型名增殖与模型选择高亮错乱复现
- rc7：本地缓存错图/路径混用（history 与 .tapnow_cache）
- rc7：黄色主题开关关闭态淡灰与高亮色块浅黄
- rc7：历史存储配额超限（tapnow_history）与自动保存核查
- rc7：本地缓存路径变更自动刷新与 history/.tapnow_cache 路径过滤
- rc7：历史存储 localStorage 上限收敛（避免配额溢出）
- rc7：API 模型测试/状态改用 _uid 防止同名覆盖
- rc7：网格点需随缩放/平移联动，当前样式不够明显（待修复）
- rc7：Base64 转 Blob 与 data:image URL 报错刷屏（待修复）
- rc7：本地缓存代理 403 与缓存失败刷屏（待修复）
- rc7：浏览选择图片/视频保存路径未生效（待修复）
- rc7：重建历史缩略图 data URL 报错（待修复）
- rc7：自动保存是否增大存储空间需说明
- rc3：批量下载卡死、分镜批量无返图、Lightbox 跳跃、403 刷屏、Base64/ERR_INVALID_URL 仍复现（待修复）
- 2.8.1：预览窗口浅黄底色、选择图片按钮深灰、分镜预览底色交替
- 2.8.1：模型库参数上限 30 + 参数备注映射 + JSON/Python 请求预览
- 2.8.1：历史模型名限 15 字节，悬浮提示显示 provider
- 2.8.1：缓存拉取仅在 provider 启用代理时走 /proxy
- 2.8.1：data URL 规范化与 ERR_INVALID_URL 报错收敛
- 2.8.1：AutoSave 优先 IndexedDB，localStorage 仅兜底
- 2.8.1：新增 /pick-path 浏览选择目录并回写缓存路径
- 2.8.1：缓存/代理/Base64 报错梳理输出 MD
- 2.8.2：选项按钮黄色系去灰（AI 绘图/视频/分镜）
- 3.8.1：黑屏报错 Cannot access '$s' before initialization（已修复待验证）
- 3.8.1：正式版本号需确认，本次黑屏修复覆盖 3.8.1
- 3.8.1：模型库参数值上限 30、备注启用复选框、预览 JSON 可编辑 + 修改覆盖
- 3.8.1：浏览选择缓存路径需同步输入框与本地配置
<!-- AG_STATE:AUTO:START -->
## 自动状态(脚本维护)
- 最近执行 2026-02-06 08:02
- 当前版本: v3.8.5
- 备份文件: src:backups\src_backup_v3.8.5_finish_task_20260206-0802.zip; localserver:backups\localserver_backup_v3.8.5_finish_task_20260206-0802.zip
- 构建产物: D:\CodingPlayground\Tapnow-Git-Sync\dist\Tapnow Studio-V3.8.5.html
- 文档检查: docs/feature_pool.md, docs/user_requirements_raw.md, changelog.md, docs/AG_STATE.md
<!-- AG_STATE:AUTO:END -->




















































































































































