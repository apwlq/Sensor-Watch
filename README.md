The Sensor Watch
================

[Sensor Watch](https://www.sensorwatch.net)는 카시오 F-91W 클래식 손목시계를 위한 기판 교체용 보드입니다. 이 보드는 세그먼트 LCD 컨트롤러가 내장된 Microchip SAM L22 마이크로컨트롤러로 구동됩니다. 제공되는 워치 라이브러리를 사용해 직접 프로그램을 작성할 수 있고, 내장된 UF2 부트로더를 통해 USB로 손쉽게 펌웨어를 업로드할 수 있습니다. 이후 기존 시계 케이스에 이 보드를 장착하면, 손목 위에서 자신만의 소프트웨어를 실행할 수 있습니다.

![image](/images/sensor-watch.jpg)

특징:
* ARM Cortex M0+ 마이크로컨트롤러
* 알람 기능을 지원하는 실시간 시계를 위한 32KHz 크리스털
* 10자리 세그먼트 LCD와 5개의 상태 표시 세그먼트
* 인터럽트가 가능한 버튼 3개
* PWM 제어가 가능한 빨강 / 초록 LED 백라이트
* 선택 사양 피에조 부저 (약간의 납땜 필요)
* 보드 내장 USB Micro B 커넥터
* 더블 탭으로 UF2 부트로더에 진입 가능한 리셋 버튼
* 9핀 플렉스 PCB 커넥터

![image](/images/sensor-board.png)

이 보드에는 센서가 없다는 점을 눈치챘을 수도 있습니다. 이는 의도적인 설계입니다. 미리 센서를 정해두는 대신, 사용자가 원하는 센서를 얹은 아주 작은 플렉서블 PCB를 추가하고, 9핀 커넥터를 통해 이를 연결하는 것이 목표입니다. 이 커넥터는 다음과 같은 전원 및 연결 옵션을 제공합니다:

* 3V 전원 (CR2016 코인 셀의 정격 전압, 약 2.7V까지 떨어질 수 있음)
* 풀업 저항이 내장된 I²C 인터페이스
* 범용 입출력(IO) 핀 5개, 다음과 같이 설정 가능:
    * 아날로그 입력 5개
    * 내부 풀업 또는 풀다운 저항을 사용하는 인터럽트 입력 디지털 핀 5개
    * 디지털 출력 5개
    * SPI 컨트롤러 (여분의 아날로그/GPIO 핀 1개 포함)
    * UART TX/RX 한 쌍 (GPIO 3개 여유)
    * 두 개의 독립적인 TC 인스턴스에서 최대 4개의 PWM 핀
    * 초저전력 BACKUP 모드에서 깨울 수 있는 외부 웨이크 입력 2개

| **핀** | **디지털** | **인터럽트** | **아날로그** | **I2C** | **SPI** | **UART** | **PWM** | **외부 웨이크** |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **A0**  | PB04 | EIC/EXTINT\[4\] | ADC/AIN\[12\] | — | — | — | — | — |
| **SCL** | — | — | — | SCL<br>SERCOM1\[1\] | — | — | — | — |
| **SDA** | — | — | — | SDA<br>SERCOM1\[0\] | — | — | — | — |
| **A1**  | PB01 | EIC/EXTINT\[1\] | ADC/AIN\[9\] | — | SCK<br>SERCOM3\[3\] | RX<br>SERCOM3\[3\] | TC3\[1\] | — |
| **A2**  | PB02 | EIC/EXTINT\[2\] | ADC/AIN\[10\] | — | MOSI<br>SERCOM3\[0\] | TX 또는 RX<br>SERCOM3\[0\] | TC2\[0\] | RTC/IN\[1\] |
| **A3**  | PB03 | EIC/EXTINT\[3\] | ADC/AIN\[11\] | — | CS<br>SERCOM3\[1\] | RX<br>SERCOM3\[1\] | TC2\[1\] | — |
| **A4**  | PB00 | EIC/EXTINT\[0\] | ADC/AIN\[8\] | — | MISO<br>SERCOM3\[2\] | TX 또는 RX<br>SERCOM3\[2\] | TC3\[0\] | RTC/IN\[0\] |

이 작은 “센서 보드”들은 정해진 외형을 가지며, 전자 부품을 배치할 수 있는 공간은 매우 작습니다(5.7 × 5.7 × 1 mm). 그럼에도 불구하고 환경 센서, MEMS 가속도계나 자기 센서, 그리고 몇 개의 디커플링 커패시터를 넣기에는 충분한 공간입니다. 다만 QFN이나 LGA 패키지 부품으로 제한될 가능성이 큽니다. SOIC는 너무 크고, SSOP 패키지도 일반적으로 두께가 너무 두껍습니다. 여러 센서 보드에 대한 참고 설계는 이 저장소의 `PCB/Sensor Boards` 디렉터리에서 확인할 수 있습니다.

Getting code on the watch
-------------------------
이 저장소에 포함된 워치 라이브러리는 아직 개발 중이지만, 시작하기에는 충분합니다. 새 프로젝트를 만들려면 apps 폴더에 있는 “starter-project” 폴더를 복사한 뒤, app.c 파일에 코드를 작성하세요.

시계를 위한 프로젝트를 빌드하려면 [GNU Arm Embedded Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads/)을 설치해야 합니다. 워치 라이브러리는 `9-2019-q4-major`와 `10.3-2021.07` 버전에서 테스트되었습니다. Debian이나 Ubuntu를 사용 중이라면 `apt install gcc-arm-none-eabi`로 충분합니다.

프로젝트를 빌드하려면 터미널에서 프로젝트의 `make` 폴더로 이동한 뒤 `make`를 실행하세요.

Sensor Watch 보드에 프로그램을 설치하려면 USB 포트에 시계를 연결한 후, 보드 뒷면의 작은 리셋 버튼을 두 번 빠르게 누르세요. LED가 빨간색으로 켜지며 점멸하기 시작할 것입니다. (그렇지 않다면 보드를 거꾸로 꽂지 않았는지 확인하세요.) 데스크톱에 “WATCHBOOT” 드라이브가 나타나면 `make install`을 실행하세요. 그러면 컴파일된 프로그램이 UF2 파일로 변환되어 시계로 복사됩니다.

Using the Movement framework
----------------------------
약간의 수정만 하고 기존 코드를 활용하고 싶다면 `movement` 디렉터리부터 시작하세요. 기본 시계 펌웨어는 다음 명령으로 빌드할 수 있습니다:

cd movement/make
make

그다음 `movement/make/build/watch.uf2` 파일을 시계에 복사하면 됩니다. 빌드되는 워치 페이스를 수정하려면 `movement_config.h`를 참고하세요.

변경 사항을 먼저 에뮬레이터에서 테스트하고 싶을 수도 있습니다. 이를 위해 [emscripten](https://emscripten.org/)을 설치한 뒤 다음을 실행하세요:

cd movement/make
emmake make
python3 -m http.server -d build-sim

마지막으로 [watch.html](http://localhost:8000/watch.html)에 접속하면 결과를 확인할 수 있습니다.

Hardware Schematics and PCBs
----------------------------

| 이름 | 색상 | 회로도 | 거버 파일 |
| ---- | ---- | ------ | -------- |
| Sensorwatch Lite | RED | [PCB/Main Boards/OSO-SWAT-B1](PCB/Main%20Boards/OSO-SWAT-B1) | [OSO-SWAT-B1-03](PCB/Main%20Boards/OSO-SWAT-B1/OSO-SWAT-B1-03.zip) |
| Sensorwatch | GREEN | [OSO-SWAT-A1-05](PCB/Main%20Boards/OSO-SWAT-A1/OSO-SWAT-A1-05.sch) (Eagle 형식) | ? |
| Sensorwatch Pro | TBD | TBD | TBD |

License
-------
프로젝트의 각 구성 요소는 서로 다른 라이선스를 따릅니다. 자세한 내용은 [LICENSE.md](LICENSE.md)를 참고하세요.

---

본 문서는 인공지능 번역기를 통해 자동 번역된 내용으로, 번역의 정확성이나 법적 효력은 보장되지 않으므로 참고용으로만 사용하시기 바랍니다.
