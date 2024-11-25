# 인공지능소프트웨어학과 EffAI Lab

## EffAI Lab 팀 구성

|[김장환](https://github.com/wodeyuzhou)|[신은호](https://github.com/neungho1)|[이유진](https://github.com/runth)|[진경은](https://github.com/JinKyungEun000)|
|:---:|:---:|:---:|:--:|
|<img src="https://github.com/user-attachments/assets/2aa22ddc-f059-43be-b66e-7215e9068d59" width="100px" height="100px"/>|<img src="https://github.com/user-attachments/assets/2aa22ddc-f059-43be-b66e-7215e9068d59" width="100px" height="100px"/>|<img src="https://github.com/user-attachments/assets/2aa22ddc-f059-43be-b66e-7215e9068d59" width="100px" height="100px"/>|<img src="https://github.com/user-attachments/assets/2aa22ddc-f059-43be-b66e-7215e9068d59" width="100px" height="100px"/>|


## On-device 기반 객체 인식 모델(YOLO) 최적화


![quantized](https://github.com/user-attachments/assets/5aa53d40-44ec-46bc-9675-c53b89496164)
### int8 양자화된 YOLOv8n Inference 영상


[분노의 질주: 홉스 & 쇼 (Fast & Furious Presents: Hobbs & Shaw, 2019)](https://youtu.be/DcowWfPWy6Y?si=dVtwDSWJpkUMau6z)



## Project Background
- ### 필요성
  이 프로젝트는 On-device 환경에서 YOLO 객체 인식 모델을 최적화하여 클라우드 의존성을 줄이고 실시간 성능을 향상시키는 것을 목표로 한다. 기존 클라우드 기반 접근 방식은 네트워크 의존성, 지연(latency) 등의 한계가 있는데, On-device 기술은 데이터를 디바이스 내에서 처리함으로써 이러한 문제를 개선할 수 있다.

- ### 기존 해결책의 문제점
  YOLO와 같은 고성능 객체 인식 모델은 높은 정확도와 빠른 속도를 제공하지만, On-device 환경에서는 다음과 같은 제약이 존재한다:

    ```
    1. 모델 크기와 연산량: 높은 계산 요구사항으로 인해 메모리와 연산 자원이 제한된 환경에서 실행이 어려움.
    2. 배터리 소비: 디바이스 내부에서 고성능 연산을 수행 시 전력 소모가 증가하여 지속 시간이 단축됨.
    3. 성능 저하: 단순한 모델 경량화는 정확도 저하를 초래할 가능성이 높음.
    ```

    이를 해결하기 위해 양자화(Quantization) 기법을 적용하여 다음을 실현하고자 한다:

    ```
    1. 모델 경량화: 모델 크기를 줄여 메모리와 연산 자원을 절약하고 On-device 환경에 적합하도록 최적화.
    2. 실시간 처리 가능성: 디바이스 연산 능력을 활용하여 처리 속도를 개선하고 실시간 객체 인식을 지원.
    3. 성능 저하 최소화: 정확도를 유지하면서 연산 속도와 자원 효율성을 극대화하여 최적의 성능 발휘.
    ```
  
## System Requirements
  - ### 라즈베리파이5 사양


    <img width="600" alt="스크린샷 2024-11-25 20 36 16" src="https://github.com/user-attachments/assets/f6df42f0-bd19-4b78-b82a-e2d8d2225f9a">

    <img width="600" alt="raspberry pi 5" src="https://github.com/user-attachments/assets/53e0c950-413f-4162-9070-f869bc881918">

  - ### YOLOv8 Nano 사용
    <img width="600" alt="스크린샷 2024-11-25 20 32 27" src="https://github.com/user-attachments/assets/882f6f81-c371-4af5-8fb1-51a76f4327e0">


## Methods
    
  - ### Quantization
  
    <p>
    <img width="460" alt="qat-training-precision" src="https://github.com/user-attachments/assets/a2f4d5e5-983f-4488-8faa-4e14e4d61902">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    <img width="300" alt="8-bit-signed-integer-quantization" src="https://github.com/user-attachments/assets/ca7f8aa6-d7db-411d-b0d3-4e558e36bc57">
    </p>

    - Quantization:
	    - 32-bit 부동소수점 데이터를 8-bit 정수로 변환하여 연산량과 모델 크기 감소.
	    - INT8 데이터는 FP32의 값 범위를 선형적으로 매핑하여 표현되어 메모리 사용량 감소.

  - ### PTQ / QAT

    <p>
    <img width="350" alt="ptq-flowchart" src="https://github.com/user-attachments/assets/50e5d0a3-666f-4266-bb65-7efd459c29ea">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    <img width="350" alt="qat-flowchart" src="https://github.com/user-attachments/assets/eb2a3193-6782-4dcb-9a77-ab9ab1da8ae1">
    </p>

    - PTQ (Post-Training Quantization):
        - 훈련된 모델을 양자화하여 32-bit 데이터를 8-bit 정수로 변환.
        - 추가적인 훈련 없이 양자화 적용, 빠르고 간단하지만 정확도 손실 가능.


    - QAT (Quantization-Aware Training):
        - 훈련 단계에서 양자화를 시뮬레이션하여 모델을 학습.
        - 양자화에 따른 정확도 손실을 최소화하며 높은 성능 유지.


## Conclusion

  - 본 프로젝트는 On-device 환경에서 YOLO 객체 인식 모델을 최적화하기 위해 양자화 기법(PTQ 및 QAT)을 적용하여 모델의 경량화와 실시간 처리 가능성을 검증하는 데 중점을 두었다. 양자화를 통해 모델 크기와 연산량을 줄이는 데는 성공했으며, 일정 수준의 실시간 처리 성능을 확인하였다. 그러나 정확도와 효율성 면에서 실질적인 활용 수준에는 미치지 못하고 있으며, 일부 제한적인 환경에서만 적용 가능한 상태이다. 향후 추가적인 최적화와 실험을 통해 정확도를 더욱 개선하고, 실제 On-device 환경에서 폭넓게 활용 가능한 모델을 완성하는 데 초점을 맞출 계획이다.
  
## Project Outcome
- ### 2025년 OO학술대회 

## 참고
[Ultralytics Github](https://github.com/ultralytics/ultralytics) </br>
[Ultralytics Documentation](https://docs.ultralytics.com/ko) </br>
[onnxruntime-inference-examples](https://github.com/microsoft/onnxruntime-inference-examples/blob/main/quantization/image_classification/cpu/ReadMe.md) </br>
[Google AI for Developers](https://ai.google.dev/edge/litert/models/post_training_quantization) </br>
[Pytorch Quantization API Reference](https://pytorch.org/docs/stable/quantization-support.html) </br>

https://pytorch.org/blog/quantization-in-practice/ </br>
https://developer.nvidia.com/blog/achieving-fp32-accuracy-for-int8-inference-using-quantization-aware-training-with-tensorrt
