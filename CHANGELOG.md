# Super Company - Changelog

## Unreleased - Founding Executive Assignment Fix

- Fixed founder appointment losing the company scope inside `create_character.on_created`; save the new character and assign it in the caller's event context instead.
- Confirm the company's actual executive before recording a successful appointment.
- Added a monthly compatibility repair that reuses an already-generated, unassigned founder from affected saves without creating duplicates or undoing normal succession.

## Unreleased - Permanent Education Network

- Added a permanent Universal Education Network unlocked at the monthly country check when the player's International Commercial Group is prosperous.
- Each owned incorporated state receives +50 percentage points of education access and +0.10% literacy growth exactly once, including later incorporations and acquisitions. The unlock and awards survive loss of prosperity and company dissolution.
- Added Simplified Chinese and English descriptions and appended compatible monthly, incorporation, and ownership-change actions.

## Version 2.0.0 - Victoria 3 1.13 Rebalance

- Consolidated the roster to eight companies, renamed the mining company's internal key to match its role, and removed MTM without a legacy-save compatibility shell.
- Redistributed MTM's useful engine, shipbuilding, port, innovation, military-tech, company-slot, and Industrialist bonuses by company role.
- Rebuilt the industrial ratios so upstream resources remain stronger than processing and downstream production, while reducing old +500% to +800% supply spikes.
- Removed duplicate group-and-building throughput stacking.
- Replaced obsolete building and modifier keys with Victoria 3 1.13 equivalents, including the singular furniture, steel mill, and shipyard modifier names.
- Removed the full vanilla goods override, restoring current vanilla goods, electricity, transportation, obsession, and convoy rules.
- Removed prestige goods based on deprecated `manowars`/`ironclads`, fixed the Marconi radio texture path, and repaired multilingual localization encoding.
- Updated metadata and documentation for Victoria 3 1.13.11.
- Reworked railways from +125% throughput into a scale-and-profit model: +25% railway throughput, +100% infrastructure construction efficiency, and -75% infrastructure employee requirements.
- Moved the International Financial Group unlock from the late-game Investment Banks technology to Central Banking.

---

## Version 1.1.0 - Prestige Goods Trigger Fix

### English

#### Fixed Issues
- **Major Fix**: Resolved prestige goods trigger mechanism for companies with multiple prestige goods
- **Root Cause**: Companies with multiple prestige goods incorrectly had `prestige_goods_trigger` sections, which prevented proper triggering logic
- **Solution**: Removed `prestige_goods_trigger` sections from all companies with multiple prestige goods

#### Technical Details
- **Before**: All companies had `prestige_goods_trigger` sections, causing conflicts with multiple prestige goods logic
- **After**: Only companies with single prestige goods retain `prestige_goods_trigger` sections
- **Reference**: Aligned with official Victoria 3 and sample mod formats

#### Affected Companies
1. **Global Automotive Industries** - 3 prestige goods (Rolls Royce Automobiles, Mercedes Engines, Michelin Rubber)
2. **International Commercial Group** - 3 prestige goods (Savile Row Suits, Campbell Soup, Blue Mountain Coffee)
3. **Global Construction Group** - 2 prestige goods (Tiffany Glass, Krupp Steel)
4. **Military Industrial Complex** - 2 prestige goods (Mauser Rifles, Nobel Explosives)
5. **International Financial Group** - 2 prestige goods (Acid Free Paper, Swiss Tools)
6. **Tech Industrial Group** - 2 prestige goods (Edison Electricity, Marconi Radio)
7. **Mineral Smelting Corporation** - 4 prestige goods (Ural Iron, Silesian Coal, Rio Tinto Sulfur, Broken Hill Lead)

#### Correct Format Implementation
```txt
# Companies with multiple prestige goods - NO prestige_goods_trigger needed
company_example = {
    possible_prestige_goods = {
        prestige_good_item1
        prestige_good_item2
        prestige_good_item3
    }
    # No prestige_goods_trigger section
}

# Companies with single prestige good - prestige_goods_trigger required
company_example2 = {
    possible_prestige_goods = {
        prestige_good_single_item
    }
    
    prestige_goods_trigger = {
        custom_tooltip = {
            text = je_prestige_goods_complete_tt
            has_variable = prestige_good_single_item_var
        }
    }
}
```

---

### 中文

#### 修复的问题
- **重大修复**：解决了拥有多个名贵商品的公司触发机制问题
- **根本原因**：拥有多个名贵商品的公司错误地包含了 `prestige_goods_trigger` 部分，导致触发逻辑无法正常工作
- **解决方案**：移除所有拥有多个名贵商品的公司的 `prestige_goods_trigger` 部分

#### 技术细节
- **修复前**：所有公司都有 `prestige_goods_trigger` 部分，导致多个名贵商品逻辑冲突
- **修复后**：只有拥有单个名贵商品的公司保留 `prestige_goods_trigger` 部分
- **参考标准**：与维多利亚3官方和示例模组格式保持一致

#### 受影响的公司
1. **全球汽车工业集团** - 3个名贵商品（劳斯莱斯汽车、梅赛德斯发动机、米其林橡胶）
2. **国际商业集团** - 3个名贵商品（萨维尔街西装、坎贝尔汤品、蓝山咖啡）
3. **环球建设集团** - 2个名贵商品（蒂芙尼玻璃、克虏伯钢铁）
4. **军工联合体** - 2个名贵商品（毛瑟步枪、诺贝尔炸药）
5. **国际金融集团** - 2个名贵商品（无酸纸、瑞士工具）
6. **科技工业集团** - 2个名贵商品（爱迪生电力、马可尼无线电）
7. **矿物冶炼集团** - 4个名贵商品（乌拉尔铁矿、西里西亚煤炭、力拓硫矿、布罗肯希尔铅矿）

#### 正确格式实现
```txt
# 拥有多个名贵商品的公司 - 不需要 prestige_goods_trigger
company_example = {
    possible_prestige_goods = {
        prestige_good_item1
        prestige_good_item2
        prestige_good_item3
    }
    # 没有 prestige_goods_trigger 部分
}

# 拥有单个名贵商品的公司 - 需要 prestige_goods_trigger
company_example2 = {
    possible_prestige_goods = {
        prestige_good_single_item
    }
    
    prestige_goods_trigger = {
        custom_tooltip = {
            text = je_prestige_goods_complete_tt
            has_variable = prestige_good_single_item_var
        }
    }
}
```

---

## Version History

### Version 1.0.0
- Initial release of Stronger Basic Companies mod
- Added 8 new basic company types with enhanced modifiers
- Implemented prestige goods system for all companies
- Added custom icons and backgrounds for each company type

### Version 1.1.0
- Fixed prestige goods trigger mechanism
- Aligned with official Victoria 3 modding standards
- Improved compatibility with game mechanics
