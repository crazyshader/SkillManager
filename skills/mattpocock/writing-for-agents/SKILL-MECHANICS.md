# Skill 机制

本文是 [`writing-for-agents`](SKILL.md) 中针对 skill 的部分，说明文档是 skill 时需要额外注意什么，包括 frontmatter、调用方式的选择，以及路由 skill。其余写作要求适用 `SKILL.md` 中的通用参考。

## 调用方式

两种选择，各自权衡常驻上下文开销与人的记忆负担：

- **模型可调用的 skill** 保留 `description`，因此 agent 可以自主触发它，其他 skill 也能调用它。用户仍然可以直接输入它的名字：模型可调用始终*包含*用户的调用权限，description 只增加 agent 的发现能力，不会剥夺人的调用权限。description 是这个 skill 的顶层上下文指针，始终保留在上下文中：以持续的常驻开销换取可被发现。如果一个模型可调用的 skill 全部内容都是参考材料，它也可以成为共享参考的存放处：其他 skill 能调用它，因此被多个 skill 需要的参考只需放在一处。机制上：不写 `disable-model-invocation`，并编写一段面向模型的 description，其中包含触发分支（`SKILL.md` 中的指针写法规则完整适用）。
- **用户可调用的 skill** 不让 agent 自主调用：没有面向模型的 description，只有用户手动输入名字才能调用，其他 skill 也无法调用它。常驻上下文开销为零，但会增加人的记忆负担：你就是那个必须记住它存在的索引。机制上：设置 `disable-model-invocation: true`；`description` 变成面向人的内容，写成一行摘要，去掉触发列表。

只有当 agent 必须自主调用这个 skill，或其他 skill 必须调用它时，才选择模型可调用。如果它只会被人手动触发，就设为用户可调用，不承担常驻开销。

两个用户可调用的 skill 都需要的共享参考，放在任何一个 skill 里都不行：二者都没有 description，互相无法触发。应把它放到 skill 系统之外的普通文件中，由任何 skill 指向。

## 按调用方式拆分

按调用方式拆分是 skill 特有的一种拆分（按顺序拆分见 `SKILL.md`）：当一个独立的触发词应当单独触发这个 skill（即你在提示词中实际使用的触发词），或者其他 skill 必须能调用它时，才拆出一个模型可调用的 skill。新增的常驻 description 会占用常驻上下文开销，因此这种独立的可触发性必须值得这份代价。

## 路由 skill

当用户可调用的 skill 多到人记不住时，积累的记忆负担可以用**路由 skill** 解决：它是一个用户可调用的 skill，列出其他 skill 以及各自的适用场景，让人只需记住一个 skill，而不是很多个。它只能提示，不能直接触发其他 skill：用户可调用的 skill 没有 description，除了人以外，没有东西能调用它们。
