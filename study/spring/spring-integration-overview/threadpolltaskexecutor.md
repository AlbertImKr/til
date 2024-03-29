# ThreaPollTaskExecutor

Spring 프레임워크에서 제공하는 스레드 풀을 관리하는 클래스입니다

## 설정

### **setCorePoolSize**:&#x20;

코어 스레드 풀의 크기를 설정합니다. 기본적으로 생성되어 유지되는 스레드의 최소 개수입니다.

### **setMaxPoolSize**:&#x20;

스레드 풀의 최대 크기를 설정합니다. 요청이 많을 때 생성될 수 있는 스레드의 최대 개수입니다.

### **setQueueCapacity**:&#x20;

작업 큐의 최대 크기를 설정합니다. 코어 스레드 풀이 가득 찬 경우, 이 큐에 작업이 대기하게 됩니다.

### **setThreadNamePrefix**:&#x20;

생성되는 스레드의 이름에 붙는 접두사를 설정합니다.

### **setRejectedExecutionHandler**:&#x20;

작업 큐와 스레드 풀이 모두 가득 찼을 때, 새로운 작업이 들어왔을 때의 처리 방식을 설정합니다.

### **setWaitForTasksToCompleteOnShutdown**:&#x20;

스레드 풀이 종료될 때 실행 중인 작업이 완료될 것인지 여부를 설정합니다.

### **setAwaitTerminationSeconds**:&#x20;

`setWaitForTasksToCompleteOnShutdown`이 `true`로 설정된 경우, 종료 전에 완료를 기다리는 시간을 초 단위로 설정합니다.

## RejectedExecutionHandler 정책

1. **AbortPolicy (기본 정책)**
   * 이 정책을 사용하면 `RejectedExecutionException`이 발생합니다. 즉, 새로운 작업이 스레드 풀에 제출될 때 스레드 풀이 종료되거나 최대 크기에 도달하면 예외가 발생합니다.
   * **장점**:
     * 문제가 발생할 때 즉각적인 피드백을 제공합니다.
     * 기본적인 정책이므로 추가 구성 없이 사용 가능합니다.
   * **단점**:
     * 작업 제출에 실패할 경우 `RejectedExecutionException` 예외가 발생하므로 이를 적절히 처리해야 합니다.
2. **CallerRunsPolicy**
   * 이 정책은 호출한 스레드에서 작업을 실행합니다. 이는 스레드 풀에서 실행되는 것보다 더 느릴 수 있지만, 추가 작업을 버리거나 예외를 발생시키지 않습니다.
   * **장점**:
     * 작업이 폐기되지 않습니다.
     * 스레드 풀의 병목 현상을 일부 완화할 수 있습니다.
     * 추가 작업을 버리거나 예외를 발생시키지 않습니다.
   * **단점**:
     * 호출한 스레드에서 작업이 실행되기 때문에 해당 스레드의 다른 활동이 블로킹될 수 있습니다.
     * 일반적인 기대(작업이 별도의 스레드에서 실행될 것)를 벗어날 수 있습니다.
3. **DiscardPolicy**
   * 새로운 작업이 스레드 풀에 제출될 때 스레드 풀이 꽉 찼다면, 그 작업을 단순히 무시합니다. 예외는 발생하지 않습니다.
   * **장점**:
     * 예외가 발생하지 않습니다.
   * **단점**:
     * 새로운 작업이 무시되므로 데이터 손실 위험이 있습니다.
4. **DiscardOldestPolicy**
   * 스레드 풀의 작업 큐에 대기 중인 가장 오래된 작업(가장 먼저 들어온 작업)을 제거하고, 새로운 작업을 큐에 추가합니다. 이렇게 하면 최근의 작업 요청을 유지하는 동시에 큐의 크기를 초과하지 않습니다.
   * **장점**:
     * 최신의 작업을 큐에 유지합니다.
     * 예외가 발생하지 않습니다.
   * **단점**:
     * 가장 오래된 작업이 삭제되므로, 이전 작업에 대한 데이터 손실 위험이 있습니다.
