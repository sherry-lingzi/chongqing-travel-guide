# 重庆漫游手册：首次发布与以后更新

网页正文只有一个文件：`public/index.html`。它自带样式、脚本、路线示意图和行程数据，下载后直接用浏览器打开，离线可看正文和地图。打开外部导航、订票与资料链接需要联网。无需安装App，不含登录功能。

## 文件用途

- `public/index.html`：旅行手册，唯一上线页面，也是离线文件。
- `wrangler.jsonc`：Cloudflare Workers静态资源发布配置，项目名为 `chongqing-travel-guide`。
- `README.md`：本说明，不会出现在旅行网页里。

网页的候选航班与游览时间均为未订、待确认或建议，不能用倒计时认定已经出票。六晚酒店均未订。所有时间明确按北京时间（Asia/Shanghai，UTC+8）处理，浏览器在国外时也使用同一个时间点。配色按北京时间7:00—18:00白天、其余时段夜间自动切换。

## 首次设置：推荐在电脑完成

以下步骤没有技术上绝对必须用电脑的环节，但手机上传文件夹和填写部署设置较容易出错，建议第1—4步都在电脑上做。以后手机也可修改GitHub文件。

1. **解压发布包**，保留`public`文件夹，不要把`index.html`移出它。
2. **新建GitHub仓库**，建议名称`chongqing-travel-guide`，默认分支使用`main`。已有GitHub账号即可；仓库可设为Private，Cloudflare经授权也可读取。网页发布后本身是公开访问的。
3. **上传文件并提交**。在仓库点击“Add file → Upload files”，将`public`文件夹、`wrangler.jsonc`和`README.md`上传到仓库根目录。页面中应能看到`public/index.html`，避免多套一层`chongqing-guide/public`。点击“Commit changes”。也可以用自己熟悉的Git方式push。
4. **Cloudflare首次授权与部署**。登录Cloudflare控制台，进入“Workers & Pages → Create application → Import a repository”，连接GitHub，授权它访问这一个仓库，然后选该仓库和`main`分支。填写如下配置，确认后部署：

   | 设置 | 填写内容 |
   |---|---|
   | Worker名称 / 项目名 | `chongqing-travel-guide`（与`wrangler.jsonc`中的name一致） |
   | 根目录 | 仓库根目录；通常留空，界面要求路径时填`/` |
   | 构建命令 | 留空：单文件静态页面无需构建 |
   | 部署命令 | `npx wrangler deploy` |
   | 生产分支 | `main` |

   Wrangler从仓库根目录读取`wrangler.jsonc`，发布`./public`。无需额外Worker业务代码、数据库或API密钥。Cloudflare的Git集成负责自动部署，无需另设GitHub Actions。
5. **等待部署成功**，从Worker页面打开系统给出的`workers.dev`地址，测试首页、每日展开地图和倒计时。这就是可分享的公开网址。不要自行猜测实际子域名。若`workers.dev`被禁用，请在项目域名与路由设置中启用。
6. **确认自动发布**。在GitHub中对`public/index.html`做一次实际内容修改并提交到`main`，在Cloudflare的Builds查看对应提交部署成功，再刷新公开网页确认更新。完成这一步，才算验证了“push即上线”。

## 以后改行程

网页底部内嵌`DATA`，保存七天的地点、时段、说明、住宿区域与资料链接。页面顶部倒计时使用`milestones`，包含明确带`+08:00`的时间字符串。

向助手发送机票、酒店或会议信息，可更新同一个`public/index.html`并提交。确认航班时间后需同步更新预订区和倒计时，调整行程后也要同步每日地图点位。不能只改一处文字。

目前已知航班和住宿都是未订，不要自行填写订单号或凭空补上航班号。公开网页不放身份证号、订单确认号、房间号、个人电话、酒店订单截图或票面二维码。酒店确定后可加入酒店名称、地址和酒店公开电话。

## 发布状态与限制

GitHub仓库为`https://github.com/sherry-lingzi/chongqing-travel-guide`，公开网址为`https://chongqing-travel-guide.xueranling32.workers.dev`。Cloudflare Workers已连接`main`分支；2026年10月2日已用一次真实push验证自动发布。以后仍应在每次重要更新后检查Cloudflare构建状态并刷新公开网页核对内容。

静态HTML上线本身不增加页面运行依赖；Cloudflare构建环境的Wrangler仅用于部署。使用免费套餐进行此类小型手册部署，额度和限制以Cloudflare当前政策为准。

## 说明与官方资料

- Cloudflare静态资源部署：https://developers.cloudflare.com/workers/static-assets/get-started/
- Cloudflare Git集成：https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/
- 网站阻碍说明：https://help.openai.com/articles/20001280-using-cloud-browser-in-chatgpt#when-a-website-blocks-the-task

地图为不按比例的顺序与区域示意图，不是道路地图。Google多点导航只提供计划点位顺序；公共交通、铁路和武隆景区接驳应分段查询。国内出行优先使用页面提供的高德地图入口核对路线。
