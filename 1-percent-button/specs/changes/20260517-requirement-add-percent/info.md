# 代码仓理解: 百分号按钮

## 需求类别
- **类型**: requirement-add
- **分析侧重**: 影响范围（已确认逻辑层无需变更）

## 仓库结构（相关部分）
```
entry/src/main/ets/
  pages/CalculatorPage.ets    ← 本次修改（+% 按钮）
  calculator/Expression.ets   ← 已有 getPercentString() 完整实现
  calculator/Calculator.ets   ← 无需变更
  calculator/NumberFormatter.ets ← 无需变更
  model/Models.ets            ← 无需变更
  model/ErrorFlags.ets        ← 无需变更
```

## 关键文件
| 文件 | 用途 | 本次影响 |
|------|------|:--:|
| CalculatorPage.ets | 主计算器 UI | modify |
| Expression.ets | 表达式预处理（含 getPercentString） | none |

## 依赖关系
```
ButtonGrid() → BtnOp('%') → onOp('%') → expression += '%'
  ↓
onEquals() → Expression.getCleanExpression(expression)
  ↓
Expression.getPercentString() → 将 "50+10%" 展开为 "50+50*(10/100)"
  ↓
CalcEngine.evaluate() → 55
```

## 关键接口
| 接口 | 签名 | 用途 |
|------|------|------|
| getPercentString | (calculation: string): string | 将 % 表达式展开为标准算式 |

## 风险点
- 首行变为 5 按钮后，小屏设备可能需要更窄的按钮宽度（16% → 已在假设中考虑）
- 无其他风险（逻辑层完全复用，无新增逻辑）
