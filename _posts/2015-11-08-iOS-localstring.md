---
layout : post
title : "iOS 本地默认叼炸天"
category : iOS
duoshuo: true
date : 2015-11-08
tags : [iOS ,localstring, oc ]
---

**设置默认语言国际化：**

<pre class="brush: oc;  ">
#define CURR_LANG                        ([[NSLocale preferredLanguages] objectAtIndex:0])   

+(NSString *)JFLocalizedString:(NSString *)translation_key {   
    
    NSString * s = NSLocalizedString(translation_key, nil);   
    
    // 除英文及法文以外，其他默认显示德文
    if (![CURR_LANG hasPrefix:@"en"] && ![CURR_LANG hasPrefix:@"fr"]) {
        NSString * path = [[NSBundle mainBundle] pathForResource:@"de" ofType:@"lproj"];
        NSBundle * languageBundle = [NSBundle bundleWithPath:path];
        s = [languageBundle localizedStringForKey:translation_key value:@"" table:nil];
    }
    return s;
}
</pre>

  注意：由于iOS9之后增加了区号，所以不适合用isEqual方法来判断本机语言，如：本机语言为英文时，iOS9之前获得的本机语言为en,而iOS9之后获得的本机语言为en-CN ,故此时使用 [CURR_LANG isEqual:@"en-CN"] 方法会出现适配不正确问题。所以建议采用获取本机语言前缀的方式来确定本机语言国际化。
