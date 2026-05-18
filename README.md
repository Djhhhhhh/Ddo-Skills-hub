# DDO Skills Hub

共享的 Agent Skills 仓库。在此获取、使用与贡献可复用的 Skill。

---

## 使用教程

任选一种方式，将 `skills/<skill-name>/` 拿到本地。

**方式 A：Git 稀疏检出（推荐，只拉单个 Skill）**

```bash
# windows powershell
git clone --depth 1 --filter=blob:none --sparse https://github.com/Djhhhhhh/Ddo-Skills-hub tmp-skill; `
cd tmp-skill; `
git sparse-checkout set skills/<skill-name>; `
git checkout; `
cd ..; `
mv tmp-skill/skills/<skill-name> ./<skill-name>; `
Remove-Item -Recurse -Force tmp-skill
# windows cmd
git clone --depth 1 --filter=blob:none --sparse https://github.com/Djhhhhhh/Ddo-Skills-hub tmp-skill ^
cd tmp-skill ^
git sparse-checkout set skills/<skill-name> ^
git checkout ^
cd .. ^
move tmp-skill\skills\<skill-name> .\<skill-name> ^
rmdir /s /q tmp-skill
# mac/linux
git clone --depth 1 --filter=blob:none --sparse https://github.com/Djhhhhhh/Ddo-Skills-hub tmp-skill && \
cd tmp-skill && \
git sparse-checkout set skills/<skill-name> && \
git checkout && \
cd .. && \
mv tmp-skill/skills/<skill-name> ./<skill-name> && \
rm -rf tmp-skill
```

**方式 B：克隆后手动复制**

```bash
git clone https://github.com/Djhhhhhh/Ddo-Skills-hub
# 复制 skills/<skill-name>/ 到你的目标位置
```

**方式 C：GitHub 网页**

1. 打开仓库 → 进入 `skills/<skill-name>/`
2. 手动复制SKILL内容到本地

---

## 如何提交 Pull Request

### 流程

1. **Fork** 本仓库（或从 `main` 拉取功能分支，按团队规范执行）
2. 创建分支：`feat/skill-<skill-name>` 或 `fix/skill-<skill-name>`
3. 用 **init-skill** 初始化目录
4. 完成编写后，使用 **validate-skill** 执行skill格式自检
5. 推送分支并创建 **Pull Request**
6. 至少 1 名维护者 Review 通过后 **Squash merge**

---

## License

[MIT](LICENSE)