# Changelog
## TapSDK v2.1.6 2020/07/01
### TapMoment
#### Optimization and fixed bugs
- 修复调用 [TapMoment close] 不生效的 bug

## TapSDK v2.1.5 2020/06/22
### TapMoment
#### New features
- 新增场景化入口内的回调 code = TM_RESULT_CODE_MOMENT_SCENE_EVENT

## TapSDK v2.1.4 2020/06/10
### TapBootstrap
#### Optimization and fixed bugs
- 内部优化

### TapFriend
#### New features
- 新增设置富信息和查询富信息接口
- TapUserRelationShip 新增 online & time & TapRichPresence 参数

### TapCommon
#### Optimization and fixed bugs
- 优化多语言相关

## v2.1.3 2020/05/28
### Feature
* 新增繁体中文、日文、韩文、泰语、印度尼西亚语5种新的翻译，并可通过 `TapBootstrap setPreferredLanguage:` 设定
* TapCommonSDK 新增方法 `TapGameUtil isTapTapInstalled` 和 `TapGameUtil isTapGlobalInstalled`，可用来检测设备上是否安装了TapTap/TapTap 国际版客户端，需要注意，这个能力需要在 info.plist 中 LSApplicationQueriesSchemes 配置里增加 `tapsdk` 和 `tapiosdk`

### Bugfix
* TapDB 修复了一个关于版本定义的问题

## v2.1.2 2020/05/14
### TapBootstrap
#### Breaking changes
* 删除 `openUserCenter` 接口

### TapFriends
#### Feature
* 新增消息回调的接口
    ``` objectivec
    + (void)registerMessageDelegate:(id<ComponentMessageDelegate>)delegate;
    ```
* 新增获取好友邀请链接的接口
    ``` objectivec
    [TapFriends generateFriendInvitationWithHandler:^(NSString *_Nullable invitationString, NSError *_Nullable error) {
        if (error) {
            NSLog(@"error:%@", error);
        } else {
            NSLog(@"url %@", invitationString);
        }
    }];
    ```

* 新增调用系统分享控件分享好友邀请链接的接口
    ``` objectivec
    [TapFriends sendFriendInvitationWithHandler:^(BOOL success, NSError *_Nullable error) {
        if (success) {
            NSLog(@"分享成功");
        } else {
            NSLog(@"分享失败 %@", error);
        }
    }];
    ```

* 新增搜索用户的接口（需要登录状态）
    ``` objectivec
    [TapFriends searchUserWithUserId:self.idField.text handler:^(TapUserRelationShip *_Nullable user, NSError *_Nullable error) {
        if (error) {
            NSLog(@"error:%@", error);
        } else {
            NSString *str = @"";

            str = [str stringByAppendingString:[self beanToString:user]];
            str = [str stringByAppendingString:@"\n\n"];

            NSLog(@"friend list %@ %@ %@ %@", str, user.isBlocked ? @"yes" : @"no", user.isFollowed ? @"yes" : @"no", user.isFollowing ? @"yes" : @"no");
        }
    }];
    ```
* 新增拦截好友邀请链接唤起的接口
    ``` objectivec
    - (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey, id> *)options {
        return [TapBootstrap handleOpenURL:url] || [TapFriends  handleOpenURL:url];
    }
    ```

### TapFriendUI
#### Feature
* 新增 TapFriendUISDK.framework 和 TapFriendResource.bundle ，需要在收到链接时直接打开预制的好友添加弹窗，请加入这两个文件

### TapCommon
* 支持性升级

## v2.1.1 2020/05/10
### TapBootstrap
#### Feature
* 新增篝火测试资格校验接口
    ``` objectivec
        [TapBootstrap getTestQualification:^(BOOL isQualified, NSError *_Nullable error) {
        if (error) {
            // 网络异常或未开启篝火测试
        } else {
            if (isQualified) {
                // 有篝火测试资格
            }
        }
    }];
    ```
* 引入TapDB库时，在 `[TapBootstrap initWithConfig:config];` 接口默认初始化 TapDB，可以通过 TapDBConfig 配置 channel、gameVersion、enable 参数
    ``` objectivec
    TapDBConfig * dbConfig = [TapDBConfig new];
    dbConfig.channel = @"channel";// 游戏渠道
    dbConfig.gameVersion = @"x.y.z"; // 游戏版本号
    dbConfig.enable = YES; // 是否开启
    tapConfig.dbConfig = dbConfig;
    ```

#### Breaking changes
* TapConfig 新增 clientSecret 字段，必填
* TapBootstrapLoginType 删除 Apple 和 Guest 类型
* 删除绑定接口
    ``` objectivec
    + (void)bind:(TapBootstrapBindType)type permissions:(NSArray *)permissions;
    ```

### TapMoment
#### Feature
* 新增打开特定页面的接口（打开场景化入口等） 
    ``` objectivec
     [TapMoment directlyOpen:mconfig page:TapMomentPageShortcut extras:@{ TapMomentPageShortcutKey: @"sceneid" }];
    ```

### TapDB
#### Feature
* 新增 IDFA 开关，默认关闭
    ``` objectivec
    // 开启 IDFA
    [TapDB setAdvertiserIDCollectionEnabled:YES];
    ```
---
### v2.0.0 2020/04/08