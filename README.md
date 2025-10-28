# 2025-10-28

- target directory를 ramdisk로 사용하면 재빌드 속도가 1초 이내로 줄어드는 것으로 파악되었음. 하지만 윈도우즈에서는 램디스크 설정이 상당히 까다로운 것 같음. rust, cargo의 workflow를 그대로 사용할 수 있다는 최고의 장점이 있으나 플랫폼의 ramdisk 지원에 많은게 달려있어서 고민이 됨. windows같은 경우 linux에서 cross-compile이 가능하니 그렇게 해도 되지 않을까? mac, linux만 지원하면 되지 않을까?
- Mac에서 CARGO_TARGET_DIR 를 2GB ramdisk에 연결하였을때 단순 주석 추가 후 재빌드가 1.25초 걸림.
- Mac에서 CARGO_TARGET_DIR 없이 재빌드하니 1초 미만 걸림. 왜 더 적게 걸리지? target 크기가 3.3GB인 것으로 보임. 4GB 램디스크 만들어서 재 테스트 필요해보임. 그래도 마찬가지임.
- 램디스크 작전은 없던걸로.
