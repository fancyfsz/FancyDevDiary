https://discussions.unity.com/t/apple-deprecating-gamecenter-playerid-property/750742/9

https://stackoverflow.com/questions/64023829/is-there-unity-2019-social-localuser-id-get-legacy-gamecenterplayerid

https://developer.apple.com/videos/play/wwdc2019/615/



1. playerID
   - 旧版标识符，同一 Game Center 账户始终生成相同的 ID。
   - 已被废弃，不再推荐使用。
2. teamPlayerID
   - **跨游戏一致性**：同一主体下的所有游戏中，使用相同的 Game Center 账户会生成相同的 `teamPlayerID`。
   - **主体迁移**：如果游戏的开发商或发布主体发生变化，`teamPlayerID` 会发生变化。
   - 适用场景：主体间的数据共享（如统一账户系统）。
3. gamePlayerID
   - **单游戏独立性**：同一 Game Center 账户，在不同的游戏中会生成不同的 `gamePlayerID`。
   - **主体独立性**：开发主体迁移不会影响 `gamePlayerID`。
   - 适用场景：游戏内的数据绑定，特别是隐私要求较高的场合。

### **变化对现有逻辑的影响**

1. **旧数据兼容性问题**
   - 服务器端的用户数据是通过 `playerID` 绑定的。
   - 升级后，客户端只提供 `teamPlayerID` 和 `gamePlayerID`，无法直接映射到旧的 `playerID`。
2. **隐私性增强导致数据隔离**
   - `gamePlayerID` 为每个游戏唯一，迁移主体也不会改变，隐私性最强，但无法跨游戏共享用户数据。
   - 如果需要跨游戏共享用户数据，必须使用 `teamPlayerID`，但这依赖于主体未迁移。

