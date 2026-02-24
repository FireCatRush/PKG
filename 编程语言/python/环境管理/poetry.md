
poetry init : 初始化一个新项目，构建toml文件

poetry add: 安装包，例如 `poetry add numpy`
	1. 安装指定精确版本 `poetry add requests==2.31.0`
	2. 使用兼容版本 `poetry add requests^2.31.0 `
		等价于：`>=2.31.0, <3.0.0`  允许安装 **2.x** 的最新补丁和小版本，但不升级到 3.0（避免破坏性变更）
	3. 限制小版本更新 `poetry add requests~2.31.0`
		等价于：`>=2.31.0, <2.32.0` 只允许最后一位（patch）变化，比如 2.31.1、2.31.2，但不会升级到 2.32.0
	4. 指定版本范围 `requests = ">=2.30.0,<3.0.0"`
	5. 安装预发布版本（alpha/beta/rc）`poetry add --allow-prereleases requests==2.32.0rc1`
	6. 从 Git 安装（指定分支/标签/commit）
			`poetry add git+https://github.com/psf/requests.git#main
			`poetry add git+https://github.com/psf/requests.git@v2.31.0`

显示指定使用的python解释器：`poetry env use <path-to-python>`

在 Poetry 中，默认情况下虚拟环境（venv）**不会创建在项目目录下**，而是统一放在系统缓存目录中，设置 `poetry config virtualenvs.in-project true` 执行后，**所有新项目**在执行 `poetry install` 或 `poetry env use ...` 时，都会在项目根目录下自动创建一个名为 `.venv` 的文件夹作为虚拟环境。
	验证方法： `poetry env info --path`