---
layout: post
title:  "Smart Lock是如何添加到Settings的"
categories: Programmer
tags: Android
author: 西夏
description: 整理记录一下Smart Lock是如何动态添加到Security Category下面的。
---

###  1，TrustAgentService中声明相关信息
参考packages/services/Car/TrustAgent/AndroidManifest.xml
{% highlight xml %}
<!-- CarUnlockService needs to be direct boot aware, since the trust agent
     binds to it during direct boot.-->
<service android:name=".CarUnlockService"
         android:directBootAware="true">
    <!-- Warning: the meta data must be included if the service is direct boot aware.
         If not included, the device will crash before boot completes. Rendering the device
         unusable. -->
    <meta-data android:name="android.service.trust.trustagent"
               android:resource="@xml/car_sample_trust_agent"/>
</service>
{% endhighlight %}

查看packages/services/Car/TrustAgent/res/xml/car_sample_trust_agent.xml
{% highlight xml %}
<trust-agent xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:priv-android="http://schemas.android.com/apk/prv/res/android"
             android:settingsActivity=".MainActivity"
             priv-android:unlockProfile="true" />
{% endhighlight %}


### 2，查看SecuritySettings中的代码
{% highlight java %}
/**
 * Important!
 *
 * Don't forget to update the SecuritySearchIndexProvider if you are doing any change in the
 * logic or adding/removing preferences here.
 */
private PreferenceScreen createPreferenceHierarchy() {
    PreferenceScreen root = getPreferenceScreen();
    if (root != null) {
        root.removeAll();
    }

	//...
		
    if (securityCategory != null) {
        maybeAddFingerprintPreference(securityCategory, UserHandle.myUserId());
        numberOfTrustAgent = addTrustAgentSettings(securityCategory);
        setLockscreenPreferencesSummary(securityCategory);
    }

	//...
		
    return root;
}
{% endhighlight %}

{% highlight java %}
// Return the number of trust agents being added
private int addTrustAgentSettings(PreferenceGroup securityCategory) {
    final boolean hasSecurity = mLockPatternUtils.isSecure(MY_USER_ID);
    ArrayList<TrustAgentComponentInfo> agents = getActiveTrustAgents(
        getActivity(), mTrustAgentManager, mLockPatternUtils, mDPM);
    for (int i = 0; i < agents.size(); i++) {
        final TrustAgentComponentInfo agent = agents.get(i);
        RestrictedPreference trustAgentPreference =
                new RestrictedPreference(securityCategory.getContext());
        // 在这里根据TrustAgent添加设置项
    }
    return agents.size();
}
{% endhighlight %}
{% highlight java %}
private static ArrayList<TrustAgentComponentInfo> getActiveTrustAgents(Context context,
    TrustAgentManager trustAgentManager, LockPatternUtils utils,
    DevicePolicyManager dpm) {
    PackageManager pm = context.getPackageManager();
    ArrayList<TrustAgentComponentInfo> result = new ArrayList<TrustAgentComponentInfo>();
    List<ResolveInfo> resolveInfos = pm.queryIntentServices(TRUST_AGENT_INTENT,
            PackageManager.GET_META_DATA);
    List<ComponentName> enabledTrustAgents = utils.getEnabledTrustAgents(MY_USER_ID);

    EnforcedAdmin admin = RestrictedLockUtils.checkIfKeyguardFeaturesDisabled(context,
            DevicePolicyManager.KEYGUARD_DISABLE_TRUST_AGENTS, UserHandle.myUserId());

    if (enabledTrustAgents != null && !enabledTrustAgents.isEmpty()) {
        for (int i = 0; i < resolveInfos.size(); i++) {
            ResolveInfo resolveInfo = resolveInfos.get(i);
            // ...
            // 在这里面查找可用的TrustAgent
            // ...
            
            /**
             *
             * 注意，因为有如下这个hard code
             * 导致只能输出一个TrustAgent
             *
             */
            if (ONLY_ONE_TRUST_AGENT) break;
        }
    }
    return result;
}
{% endhighlight %}
{% highlight java %}
// Only allow one trust agent on the platform.
private static final boolean ONLY_ONE_TRUST_AGENT = true;
{% endhighlight %}
<!-- 后面是文章参考资料 -->
