---
title: '规范commit信息和lint代码'
---

# 规范commit信息和lint代码

## 新建仓库

```bash
nest new learn-nest
cd learn-nest
```

## 安装相关依赖

```bash
npm i -D husky # trigger git hooks

npm i -D lint-staged # lint code before commit

npm i -D @commitlint/config-conventional @commitlint/cli # lint commit message
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js # set up config file for commitlint

npm i -g commitizen # propmt to fill out any required commit fields at commit time
commitizen init cz-conventional-changelog --save-dev # initialize your project to use the cz-conventional-changelog adapter by typing
```

## 配置package.json

```json
// 配置lint-staged
{
  "lint-staged": {
    "{src,test}/**/*.ts": "npm run lint"
  }
}
```

```json
// 配置husky
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  }
}
```

