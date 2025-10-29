# 2025-10-29
- `RUSTFLAGS="--emit mir"  cargo check` 로 재빌드하면 2~3초 정도 걸림. 그냥 cargo check은 언더1초. 모든 crate의 mir를 다시 생성하는 느낌이 들어서 ls -alh로 확인해보니 그렇지 않았음. tower-defense의 mir만 재생성하는데 mir 크기가 11M나 함. llvm ir이나 bc도 20mb 이상임...
- cargo expand로 펼친 tower-defense 코드는 2.1MB임. 흠... 아무래도 rust코드를 바로 js로 바꾸는 것을 도전해봐야겠음.
- memory 구조를 어떻게 할 것인가? wasm식으로 메모리를 가져갈 것인가, 아니면 js gc를 이용할 것인가? 후자의 경우 코드에서 포인터를 사용하면 난감해짐. 전자의 경우 sizeof struct 를 알아내야함.
- struct, fn 다 generic도 arg으로서 중요함. 예를들면 size_of의 경우 generic_args가 크기를 결정함. struct = fields + generic_args, fn = args + generic_args + ret + statemenets. 이런식으로 생각해야하지 않을까 싶음.

# 2025-10-28

- target directory를 ramdisk로 사용하면 재빌드 속도가 1초 이내로 줄어드는 것으로 파악되었음. 하지만 윈도우즈에서는 램디스크 설정이 상당히 까다로운 것 같음. rust, cargo의 workflow를 그대로 사용할 수 있다는 최고의 장점이 있으나 플랫폼의 ramdisk 지원에 많은게 달려있어서 고민이 됨. windows같은 경우 linux에서 cross-compile이 가능하니 그렇게 해도 되지 않을까? mac, linux만 지원하면 되지 않을까?
- Mac에서 CARGO_TARGET_DIR 를 2GB ramdisk에 연결하였을때 단순 주석 추가 후 재빌드가 1.25초 걸림.
- Mac에서 CARGO_TARGET_DIR 없이 재빌드하니 1초 미만 걸림. 왜 더 적게 걸리지? target 크기가 3.3GB인 것으로 보임. 4GB 램디스크 만들어서 재 테스트 필요해보임. 그래도 마찬가지임.
- 램디스크 작전은 없던걸로.
- Mir -> js 작업을 다시 진행. llvm-ir을 js로 옮기면서 느꼈던 것들을 mir쪽에 녹여서 진행.
- rhs에 const value라던가, _1 = [move _2, const 2_i32, const 3_i32] 같은 사례가 있다. rhs의 타입도 항상 {ptr,size}로 하려고 했으나, 이런 const를 지원하려면 bytes로 해야하지 않을까 싶다. stack-heap과 다른 독립적인 memory의 typedarray를 만들어서 거기에 데이터를 완성시켜 rhs로 전달하거나, 아니면 임의로 stack allocate를 하여 우항을 만들어서 제공하거나. 전자는 ptr를 얻을 수 없다. 후자는 stack deallocate만 잘 하면 될 것 같다. 고민.
- TODO: cargo rustc emit mir가 얼마나 걸리는지 확인해볼 것. 
