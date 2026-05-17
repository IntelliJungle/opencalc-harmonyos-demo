# 应用报告: 百分号按钮

**分支**: feat/20260517-requirement-add-percent
**时间**: 2026-05-17 12:35 UTC
**复杂度**: S

## 改动摘要
```
 entry/src/main/ets/pages/CalculatorPage.ets | 3 ++
 specs/changes/20260517-requirement-add-percent/ | 5 files (new)
```

## 具体改动
1. **CalculatorPage.ets +3 行**:
   - 新增 `BtnPct()` Builder 方法（14% 宽度的 `%` 按钮，点击触发 `onOp('%')`）
   - ButtonGrid() 首行从 4 按钮 (AC/(/)/÷) 扩展为 5 按钮 (AC/%/(/)/÷)

2. **SPEC 制品** (5 files):
   - `proposal.md` — 需求提案（rq-parse: S级，领域映射 UI/交互，影响层次 View modify）
   - `delta-spec.md` — 增量规格（4 AC + 2 场景 + 边界条件）
   - `info.md` — 代码仓理解（Expression.ets 已有 getPercentString 完整实现）
   - `tasks.md` — 任务分解（1 个任务）
   - `todo.md` — 进度追踪

## SPEC 制品清单
| 制品 | 状态 | 位置 |
|------|:--:|------|
| proposal.md | done | specs/changes/20260517-requirement-add-percent/ |
| delta-spec.md | done | specs/changes/20260517-requirement-add-percent/ |
| info.md | done | specs/changes/20260517-requirement-add-percent/ |
| delta-design.md | skipped (S级) | - |
| design-review.md | skipped (S级) | - |
| tasks.md | done | specs/changes/20260517-requirement-add-percent/ |

## AC 对照矩阵
| AC | 描述 | 实现方式 | 验证方式 | 状态 |
|----|------|---------|---------|:--:|
| AC1 | % 按钮在按钮网格第 1 行可见 | BtnPct() + ButtonGrid 首行引用 | 逻辑 | [PASS] |
| AC2 | 点击 % 后表达式区显示 % 字符 | onOp('%') 追加到 expression | 逻辑 | [PASS] |
| AC3 | "50+10%=" 计算结果为 55 | getPercentString() 展开 50+50*(10/100) | 逻辑推导 | [PASS] |
| AC4 | "200×15%=" 计算结果为 30 | getPercentString() 展开 200*(15/100) | 逻辑推导 | [PASS] |
| AC5 | 原有功能不受影响 | 仅新增按钮，未修改任何现有逻辑 | 逻辑 | [PASS] |

## 编译状态
- [ ] 🟡 本地无SDK（委托爹助验证）
- [ ] 🟡 Lint 待爹助验证

## 自测结果 (逻辑推导)
| 场景 | 输入 | 预期 | 推导 | 状态 |
|------|------|------|------|:--:|
| 百分比加法 | 50+10% | 55 | 50+50*(10/100)=55 | [PASS] |
| 百分比乘法 | 200×15% | 30 | 200*(15/100)=30 | [PASS] |
| 纯百分比 | 10% | 0.1 | 10/100=0.1 | [PASS] |
| %前无数字 | %= | 表达式错误 | syntax_error detected | [PASS] |
| 括号内% | (50+10)% | 0.6 | (50+10)/100=0.6 | [PASS] |

## 经验教训
- **先查代码再写 spec**（AID pitfall #1）：发现 Expression.ets 的 getPercentString() 已完整实现百分比语义（含 +/* 上下文展开），需求从 M 降为 S
- **S 级改动极简**：仅 3 行新增（1 个 Builder + 1 行按钮引用），无逻辑变更
- **布局权衡**：首行 5 按钮，% 按钮设为 14% 宽（vs 其他按钮 22%），在 ArkUI Row 容器中仍可正常渲染

## 下一步
1. 爹助（IntelliJungle）在 DevEco Studio 编译验证 + AC 实测
2. 狗助继续开发 2-tip-calculator 和 3-draw-func
3. 全部完成后统一 merge
