### **func** initializePlayer()

- NSDataAsset 을 이용하여 sound 파일을 가지고 온다. 해당 파일이 없을 수도 있기 때문에 guard let 을 통해 nil 일 경우의 처리를 해준다.
- 가지고 온 파일을 재생하기 위한 AVAudioPlayer 객체 생성, 객체 생성시 실패할수도 있기 때문에 do-try-catch 구문을 통해 예외 처리를 해준다.
- 성공적으로 가져왔으면 delegate 작성을 해준다.
- 생성된 AVAudioPlayer의 사운드의 총 재생 시간인 duration 프로퍼티를 이용하여 slider 값을 설정해준다.

### func updateTimeLabelText()

- 재생되는동안, 시간 표시 업데이트 해준다.

### **func** makeAndFireTimer()

- Timer 클래스는 일정한 시간 간격이 지나면 지정된 메시지를 특정 객체로 전달하는 기능 제공한다.

```swift
class func scheduledTimer(withTimeInterval: TimeInterval, repeats: Bool, block: (Timer) -> Void)
```

- 갱신해줘야 하는 시간: TimeInterval, 반복 여부 : repeats, 수행할 작업: block
- timer.fire() 을 통해 타이머 시작

```swift
if self.progressSlider.isTracking { return }
            
self.updateTimeLabelText(time: self.player.currentTime)
self.progressSlider.value = Float(self.player.currentTime)
```

- 만약 재생 중, Slider 의 값을 변경한다면, sliderValueChanged() 에서 해당 위치의 시간으로 label 의 값을 변경해준다.
- 그냥 재생 시켰을 때는 초단위로 label 변경

### **func** invalidateTimer()

- 타이머가 종료되었을때, 메모리 회수

### 코드상에서 UI 구성

- **func** addViewsWithCode()
- **func** addPlayPauseButton()
- **func** addTimeLabel()
- **func** addProgressSlider()    

### **@IBAction** **func** touchUpPlayPauseButton(_ sender: UIButton)

```swift
sender.isSelected = !sender.isSelected
```

- isSelected 는 자동으로 바뀌는 변수가 아니기때문에, 직접 설정해줘야함
- 버튼이 눌렸을때 isSelected 가 true 로 자동으로 바뀌는게 아니다. 그냥 항상 false

### **@IBAction** **func** sliderValueChanged(_ sender: UISlider)

- Slider 의 값이 변했을 때, 해당 시간의 소리를 재생해준다.  

```swift
if sender.isTracking { return }
```

- 위 코드는 슬라이더 값을 변경할 동안에는 기존 재생을 이어가다가 슬라이더가 한 위치에 정지했을때, 그 위치의 소리를 재생하기 위한 것

### AVAudioPlayerDelegate

- **func** audioPlayerDecodeErrorDidOccur(_ player: AVAudioPlayer, error: Error?)
    
    → 에러 발생시 처리 코드
    
- **func** audioPlayerDidFinishPlaying(_ player: AVAudioPlayer, successfully flag: Bool)
    
    → 재생이 끝났을 때 처리 코드
