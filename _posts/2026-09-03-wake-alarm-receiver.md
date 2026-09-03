---
layout: post
kind: worklog
title: "알람은 걸려 있었는데 받을 리시버가 없었다"
description: "예약은 걸려 있고 시스템은 시각에 맞춰 쐈는데 알림은 0건이었다. 받을 컴포넌트가 앱에 선언돼 있지 않았다."
date: 2026-09-03 09:00:00 +0900
path_label: "notes / android / notification"
summary_label: "STATUS"
summary_body: "리시버 선언으로 발화 복구. Doze·제조사 최적화 아래 발화는 미판정."
evidence:
  - ref: "AndroidManifest.xml"
    note: "리시버 2개 선언(exported=false) + 부팅 인텐트 필터"
  - ref: "flutter_local_notifications 22.0.1 README:381"
    note: "앱이 직접 선언해야 예약 알림이 뜬다는 요구"
  - ref: "dumpsys alarm / dumpsys notification"
    note: "발화 기록 있음 대 알림 0건"
---

"설정은 켜져 있는데 아침에 안 울린다." 만들고 있는 안드로이드 앱의 기상 알람을 두고 이런 보고를 받았다. 앱 설정 화면은 예약이 걸려 있다는 안내를 계속 띄우고 있었고, 확인해 보니 예약 자체도 실제로 걸려 있었다.

폰을 붙여 발화 시각 전후를 들여다봤다. `AlarmManager`에는 알람을 쐈다는 기록이 남아 있는데 같은 시각의 알림은 0건이었고, 크래시도 예외 로그도 없었다. 예약은 제대로 걸렸고 시스템은 시각에 맞춰 제 할 일을 했는데 결과만 사라진 셈이라, 앱 안쪽에서는 볼 게 아무것도 없었다.

`adb shell dumpsys alarm`의 앱 패키지 대목과 `adb shell dumpsys notification`의 같은 발화 건을 나란히 놓고 봤다. 앞쪽에는 예약이 등록돼 있고 발화 기록도 "쐈다"로 남아 있는데, 뒤쪽에는 이 앱이 올린 알림이 0건이었다.

`flutter_local_notifications`의 예약 발화는 앱 프로세스 밖에서 온다. `AlarmManager`가 브로드캐스트를 쏘고 그걸 `BroadcastReceiver`가 받아 알림을 띄우는 구조라, 앱이 죽어 있어도 시스템이 깨울 수 있게 리시버가 매니페스트에 정적으로 선언돼 있어야 한다. 앱 매니페스트에는 알림 관련 권한이 7종 선언돼 있고, 리시버는 한 개도 없었다.

![발화 경로 전/후 도식. (전) 예약에서 브로드캐스트까지는 이어지지만 받을 리시버가 없어 알림이 0건으로 끊긴다. (후) 같은 경로 끝에 선언된 리시버가 붙어 알림까지 이어진다.](/assets/img/wake-alarm-receiver-before-after.svg)
*발화 경로 전/후. 끊긴 자리는 예약도 브로드캐스트도 아니고 그걸 받을 컴포넌트였다.*

그럼 리시버는 누가 넣어야 하나. 플러그인 패키지(22.0.1) 안의 매니페스트에는 `uses-permission` 두 줄이 전부였고 리시버 네 개는 example 앱 쪽에만 있었는데, 같은 버전 `README.md` 381행이 `<application>` 태그 사이에 리시버 두 개를 선언해야 예약 알림이 실제로 뜬다고 적어 두었다. 앞 세션에서 나는 이 대목을 보지 않았다. 플러그인을 붙이면 필요한 네이티브 선언은 라이브러리가 자기 매니페스트에 넣어 왔을 것이라고 전제한 채 Dart 쪽만 훑었다.

```xml
<!-- AndroidManifest.xml — <application> 안. 플러그인 패키지 경로는 줄여 적었다 -->
<receiver android:name=".../ScheduledNotificationReceiver"
          android:exported="false" />

<receiver android:name=".../ScheduledNotificationBootReceiver"
          android:exported="false">
  <intent-filter>
    <action android:name="android.intent.action.BOOT_COMPLETED" />
    <action android:name="android.intent.action.MY_PACKAGE_REPLACED" />
    <action android:name="android.intent.action.QUICKBOOT_POWERON" />
    <action android:name="com.htc.intent.action.QUICKBOOT_POWERON" />
  </intent-filter>
</receiver>
```

설치한 뒤 다시 계측하니 병합된 매니페스트에 리시버 두 개가 들어 있었고, 설치 직후 `MY_PACKAGE_REPLACED` 복원이 밀려 있던 예약을 되살려 즉시 발화했다. 발화가 시작되자 진동 모드에서 소리가 죽는 것부터 잠금화면 뒤에 갇힌 알람 화면까지 몇 가지가 더 나왔는데, 그건 전부 리시버가 붙은 다음에야 볼 수 있는 문제였다.

애초에 이번 실패를 만든 자리는 그대로 남아 있다. 플러그인이 README로 요구하는 선언을 앱이 빠뜨렸을 때 그걸 알려주는 가드는 없고, 빌드도 되고 예약도 걸린 채로 알림만 조용히 사라진다. Doze나 제조사 배터리 최적화가 걸린 상태에서 울리는지는 아직 판정하지 못했다.
