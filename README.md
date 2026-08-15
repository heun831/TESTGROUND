# TESTGROUND — GMA Next-Gen SDK + Charles SSL

[googleads/gma-next-gen-sdk-android-examples](https://github.com/googleads/gma-next-gen-sdk-android-examples) 예제에
[Charles에서 모바일 앱 SSL 프록시를 사용하도록 설정](https://developers.google.com/admob/android/charles?hl=ko#enable_ssl_proxy_for_your_mobile_app)
을 적용한 저장소입니다.

디버그 빌드에서만 사용자가 설치한 SSL 인증서(Charles CA)를 신뢰합니다. 릴리스 APK에는 적용되지 않습니다.

## 디버그 APK 다운로드

- **Kotlin NextGenExample (debug, Charles SSL 신뢰):** [NextGenExample-debug.apk](https://github.com/heun831/TESTGROUND/raw/cursor/charles-ssl-proxy-b10e/apk/NextGenExample-debug.apk)

설치 후 기기/에뮬레이터에 Charles SSL 인증서를 설치하고 프록시를 연결하면 광고 요청 HTTPS 트래픽을 Charles에서 볼 수 있습니다.

## Charles SSL 설정 (앱 쪽)

1. `network_security_config.xml`에서 디버그 빌드만 사용자 CA를 신뢰하도록 선언합니다.

```xml
<network-security-config>
   <debug-overrides>
       <trust-anchors>
           <!-- Trust user added CAs while debuggable only -->
           <certificates src="user" />
       </trust-anchors>
   </debug-overrides>
</network-security-config>
```

적용 위치:

- `kotlin/NextGenExample/app/src/main/res/xml/network_security_config.xml`
- `java/NextGenExample/app/src/main/res/xml/network_security_config.xml`

2. `AndroidManifest.xml`의 `<application>`에 네트워크 보안 설정을 연결합니다.

```xml
<application
    android:networkSecurityConfig="@xml/network_security_config"
    ... >
```

적용 위치:

- `kotlin/NextGenExample/app/src/main/AndroidManifest.xml`
- `java/NextGenExample/app/src/main/AndroidManifest.xml`

## Charles / 기기 쪽 체크리스트

공식 가이드: [Charles 프록시 설정](https://developers.google.com/admob/android/charles?hl=ko)

1. 기기에서 Google Play 서비스를 최신으로 업데이트합니다 (14.5.74 이상).
2. 개발자 옵션을 켠 뒤 **Google > 광고 > 광고 디버그 로깅 사용**을 켭니다.
3. 컴퓨터에 Charles를 설치하고, 기기/에뮬레이터에 Charles SSL 인증서를 설치합니다.
4. 프록시를 설정합니다.
   - 에뮬레이터: `127.0.0.1` + Charles 포트
   - 실기기: 같은 Wi-Fi에서 Charles **Help > Local IP Address** + Charles 포트
5. 이 디버그 APK를 설치하고 앱을 실행한 뒤 Charles에서 광고 요청을 확인합니다.

## 로컬에서 디버그 APK 다시 빌드

```bash
cd kotlin/NextGenExample
./gradlew :app:assembleDebug
# 출력: app/build/outputs/apk/debug/app-debug.apk
```

## 라이선스

예제 소스 원본은 Google의 Apache 2.0 라이선스입니다. `LICENSE`를 참고하세요.
