# Solution Docs for KAIST CS101 HW2

> Ryan Donghan Kwon @ School of Medicine (MRC), Sungkyunkwan University

## [Task 1] Image Strip Removal (15 Pts)

### 문제 개요
이 과제에서는 이미지에서 가로 또는 세로 방향의 픽셀 줄을 제거하고, 남은 픽셀들을 이동시켜 빈 공간을 채우는 함수를 구현합니다. 제거할 영역은 무작위로 선택되며, 크기는 1픽셀부터 이미지의 전체 너비 또는 높이까지 다양하게 변할 수 있습니다.

- "horizontal" 방향: 여러 행(rows)을 제거
- "vertical" 방향: 여러 열(columns)을 제거

cut_start부터 cut_end까지의 범위(둘 다 포함)에 해당하는 행이나 열을 제거해야 합니다.

### 해결 방법

```python
def remove_image_section(input_image, cut_start, cut_end, direction):
    width, height = input_image.size()
    
    if direction == "vertical":
        # 세로 방향 제거 시 새 이미지 크기 계산
        cut_width = cut_end - cut_start + 1
        new_width = width - cut_width
        new_height = height
        
        # 새 이미지 생성
        new_image = create_picture(new_width, new_height)
        
        # 제거할 부분 이전의 픽셀들 복사
        for y in range(height):
            for x in range(cut_start):
                new_image.set(x, y, input_image.get(x, y))
                
        # 제거할 부분 이후의 픽셀들 복사 (왼쪽으로 이동)
        for y in range(height):
            for x in range(cut_end + 1, width):
                new_x = x - cut_width
                new_image.set(new_x, y, input_image.get(x, y))
                
    else:  # direction == "horizontal"
        # 가로 방향 제거 시 새 이미지 크기 계산
        cut_height = cut_end - cut_start + 1
        new_width = width
        new_height = height - cut_height
        
        # 새 이미지 생성
        new_image = create_picture(new_width, new_height)
        
        # 제거할 부분 이전의 픽셀들 복사
        for y in range(cut_start):
            for x in range(width):
                new_image.set(x, y, input_image.get(x, y))
                
        # 제거할 부분 이후의 픽셀들 복사 (위로 이동)
        for y in range(cut_end + 1, height):
            for x in range(width):
                new_y = y - cut_height
                new_image.set(x, new_y, input_image.get(x, y))
    
    return new_image
```

### 코드 설명

1. **이미지 크기 계산**:
   - `input_image.size()`를 통해 원본 이미지의 너비와 높이를 얻습니다.

2. **세로 방향 제거 시**:
   - 제거할 영역의 너비를 계산합니다: `cut_width = cut_end - cut_start + 1`
   - 새 이미지의 너비는 기존 너비에서 제거된 너비를 뺀 값이 됩니다.
   - 새 이미지의 높이는 원본과 동일합니다.
   - 이미지의 픽셀을 두 부분으로 나누어 처리합니다:
     - cut_start 이전의 픽셀은 그대로 복사합니다.
     - cut_end 이후의 픽셀은 왼쪽으로 이동하여 복사합니다.

3. **가로 방향 제거 시**:
   - 제거할 영역의 높이를 계산합니다: `cut_height = cut_end - cut_start + 1`
   - 새 이미지의 너비는 원본과 동일합니다.
   - 새 이미지의 높이는 기존 높이에서 제거된 높이를 뺀 값이 됩니다.
   - 이미지의 픽셀을 두 부분으로 나누어 처리합니다:
     - cut_start 이전의 픽셀은 그대로 복사합니다.
     - cut_end 이후의 픽셀은 위로 이동하여 복사합니다.

4. **픽셀 이동 로직**:
   - 세로 방향: `new_x = x - cut_width`를 통해 오른쪽 부분을 왼쪽으로 이동합니다.
   - 가로 방향: `new_y = y - cut_height`를 통해 아래쪽 부분을 위로 이동합니다.

## [Task 2] Basic Image Transformation (45 Pts)

### Task 2.1: Vertically & Horizontally Flip Images (15 Pts)

#### 문제 개요
주어진 방향에 따라 이미지를 수직 또는 수평으로 뒤집는 함수를 구현합니다. "vertical" 방향이면 이미지를 상하로 뒤집고, "horizontal" 방향이면 이미지를 좌우로 뒤집습니다.

#### 해결 방법

```python
def reflect_image(input_image, direction):
    width, height = input_image.size()
    output_image = create_picture(width, height)
    
    if direction == "horizontal":
        for y in range(height):
            for x in range(width):
                output_image.set(width - 1 - x, y, input_image.get(x, y))
    else:  # direction == "vertical"
        for y in range(height):
            for x in range(width):
                output_image.set(x, height - 1 - y, input_image.get(x, y))
    
    return output_image
```

#### 코드 설명

1. **초기화**:
   - 원본 이미지의 너비와 높이를 얻어옵니다.
   - 동일한 크기의 새 이미지를 생성합니다.

2. **수평 뒤집기(horizontal)**:
   - 각 픽셀의 x 좌표를 뒤집어 새 이미지에 설정합니다.
   - 수식 `width - 1 - x`를 사용하여 픽셀의 좌우를 반전시킵니다.
   - y 좌표는 변경되지 않습니다.

3. **수직 뒤집기(vertical)**:
   - 각 픽셀의 y 좌표를 뒤집어 새 이미지에 설정합니다.
   - 수식 `height - 1 - y`를 사용하여 픽셀의 상하를 반전시킵니다.
   - x 좌표는 변경되지 않습니다.

### Task 2.2: Rotate Images 90 Degrees (15 Pts)

#### 문제 개요
주어진 각도로 이미지를 회전시키는 함수를 구현합니다. 회전 각도는 90도의 배수여야 하며, 양수는 시계 방향, 음수는 반시계 방향 회전을 의미합니다.

#### 해결 방법

```python
def rotate_image(input_image, angle):
    width, height = input_image.size()
    
    # 각도를 0~270 범위로 정규화
    angle = angle % 360
    if angle < 0:
        angle = (angle + 360) % 360
    
    if angle == 0:
        # 회전 없음
        return input_image
    elif angle == 90:
        # 시계 방향 90도 회전
        output_image = create_picture(height, width)
        for y in range(height):
            for x in range(width):
                output_image.set(height - 1 - y, x, input_image.get(x, y))
    elif angle == 180:
        # 180도 회전
        output_image = create_picture(width, height)
        for y in range(height):
            for x in range(width):
                output_image.set(width - 1 - x, height - 1 - y, input_image.get(x, y))
    else:  # angle == 270
        # 시계 방향 270도 회전 (반시계 방향 90도)
        output_image = create_picture(height, width)
        for y in range(height):
            for x in range(width):
                output_image.set(y, width - 1 - x, input_image.get(x, y))
    
    return output_image
```

#### 코드 설명

1. **각도 정규화**:
   - 모든 각도를 0~359 범위로 정규화합니다: `angle = angle % 360`
   - 음수 각도는 양수로 변환합니다: `angle = (angle + 360) % 360`

2. **각도별 회전 로직**:
   - **0도**: 변환 없이 원본 이미지 반환
   - **90도**: 
     - 이미지 크기를 (높이, 너비)로 변경 (가로세로 교체)
     - 좌표 변환: `(x, y) → (height - 1 - y, x)`
   - **180도**: 
     - 이미지 크기 유지
     - 좌표 변환: `(x, y) → (width - 1 - x, height - 1 - y)`
   - **270도**: 
     - 이미지 크기를 (높이, 너비)로 변경 (가로세로 교체)
     - 좌표 변환: `(x, y) → (y, width - 1 - x)`

3. **좌표 변환 이론**:
   - 90도 회전: 반시계 방향으로 90도 회전 후 좌우 반전
   - 180도 회전: 상하 반전 후 좌우 반전
   - 270도 회전: 시계 방향으로 90도 회전 후 좌우 반전

### Task 2.3: Translate Images with Wrap Around (15 Pts)

#### 문제 개요
이미지를 지정된 픽셀 수만큼 수평 및 수직으로 이동시키는 함수를 구현합니다. 이때 이미지 경계를 벗어나는 픽셀은 반대편 경계에서 나타나는 wrap-around 효과를 적용해야 합니다.

#### 해결 방법

```python
def translate_image(input_image, shift_x, shift_y):
    width, height = input_image.size()
    output_image = create_picture(width, height)
    
    # 이동값을 이미지 크기로 모듈로 연산하여 최적화
    shift_x = shift_x % width
    shift_y = shift_y % height
    
    # 출력 이미지의 각 픽셀 처리
    for new_y in range(height):
        for new_x in range(width):
            # 원본 픽셀 위치 계산 (역매핑)
            orig_x = (new_x - shift_x) % width
            orig_y = (new_y - shift_y) % height
            
            # 원본 위치에서 대상 위치로 색상 복사
            output_image.set(new_x, new_y, input_image.get(orig_x, orig_y))
    
    return output_image
```

#### 코드 설명

1. **이동값 최적화**:
   - 이동값을 이미지 크기로 모듈로 연산하여 최적화합니다: `shift_x = shift_x % width`
   - 이렇게 하면 이미지 크기보다 큰 이동도 효율적으로 처리할 수 있습니다.

2. **역매핑 방식 사용**:
   - 출력 이미지의 각 픽셀 위치에 대해 원본 이미지의 어떤 픽셀이 해당 위치로 이동하는지 계산합니다.
   - `(new_x - shift_x) % width`와 `(new_y - shift_y) % height`를 사용하여 원본 픽셀 위치를 찾습니다.
   - 음수 이동에도 모듈로 연산이 올바르게 작동하도록 합니다.

3. **Wrap-around 구현**:
   - 모듈로 연산(`%`)을 사용하여 이미지 경계를 벗어나는 값을 반대편으로 감싸줍니다.
   - 이를 통해 이미지의 오른쪽 경계를 벗어난 픽셀은 왼쪽에서 다시 나타나고, 아래쪽 경계를 벗어난 픽셀은 위쪽에서 다시 나타납니다.

## [Task 3] Image Mosaic (40 Pts)

### Task 3.1: Basic Mosaic (20 Pts)

#### 문제 개요
이미지를 정사각형 블록으로 나누고, 각 블록을 블록의 왼쪽 상단 픽셀 색상으로 채우는 모자이크 함수를 구현합니다. 이미지 크기가 블록 크기로 나누어 떨어지지 않는 경우에도 처리할 수 있어야 합니다.

#### 해결 방법

```python
def mosaic_simple(input_image, block_size):
    width, height = input_image.size()
    output_image = create_picture(width, height)
    
    # 전체 블록 및 가장자리 처리
    for y_start in range(0, height, block_size):
        for x_start in range(0, width, block_size):
            # 실제 블록 크기 계산 (가장자리에서는 더 작을 수 있음)
            actual_width = min(block_size, width - x_start)
            actual_height = min(block_size, height - y_start)
            
            # 블록의 왼쪽 상단 픽셀 색상 가져오기
            block_color = input_image.get(x_start, y_start)
            
            # 블록 전체를 이 색상으로 채우기
            for y_offset in range(actual_height):
                for x_offset in range(actual_width):
                    output_image.set(x_start + x_offset, y_start + y_offset, block_color)
    
    return output_image
```

#### 코드 설명

1. **블록 순회**:
   - `range(0, height, block_size)` 및 `range(0, width, block_size)`를 사용하여 블록 단위로 이미지를 순회합니다.
   - 각 블록의 시작점 좌표는 (x_start, y_start)입니다.

2. **가장자리 처리**:
   - 실제 블록 크기를 계산합니다: `actual_width = min(block_size, width - x_start)`
   - 이를 통해 이미지 가장자리에 있는 불완전한 블록도 올바르게 처리합니다.

3. **색상 적용**:
   - 블록의 왼쪽 상단 픽셀 (x_start, y_start)의 색상을 가져옵니다.
   - 해당 블록의 모든 픽셀에 동일한 색상을 적용합니다.
   - 중첩된 루프를 사용하여 블록 내의 모든 픽셀을 처리합니다.

### Task 3.2: Average Mosaic (20 Pts)

#### 문제 개요
이미지를 정사각형 블록으로 나누고, 각 블록을 블록 내 모든 픽셀의 평균 색상으로 채우는 모자이크 함수를 구현합니다. 이미지 크기가 블록 크기로 나누어 떨어지지 않는 경우에도 처리할 수 있어야 합니다.

#### 해결 방법

```python
def mosaic_average(input_image, block_size):
    width, height = input_image.size()
    output_image = create_picture(width, height)
    
    # 전체 블록 및 가장자리 처리
    for y_start in range(0, height, block_size):
        for x_start in range(0, width, block_size):
            # 실제 블록 크기 계산 (가장자리에서는 더 작을 수 있음)
            actual_width = min(block_size, width - x_start)
            actual_height = min(block_size, height - y_start)
            
            # 이 블록의 평균 색상 계산
            total_r, total_g, total_b = 0, 0, 0
            pixel_count = 0
            
            # 블록 내의 모든 픽셀 값 합산
            for y_offset in range(actual_height):
                for x_offset in range(actual_width):
                    r, g, b = input_image.get(x_start + x_offset, y_start + y_offset)
                    total_r += r
                    total_g += g
                    total_b += b
                    pixel_count += 1
            
            # 평균 계산 (정수 나눗셈 사용)
            avg_r = total_r // pixel_count
            avg_g = total_g // pixel_count
            avg_b = total_b // pixel_count
            avg_color = (avg_r, avg_g, avg_b)
            
            # 블록 전체를 평균 색상으로 채우기
            for y_offset in range(actual_height):
                for x_offset in range(actual_width):
                    output_image.set(x_start + x_offset, y_start + y_offset, avg_color)
    
    return output_image
```

#### 코드 설명

1. **블록 순회**:
   - task 3.1과 동일하게 블록 단위로 이미지를 순회합니다.
   - 각 블록의 실제 크기를 계산하여 가장자리 블록도 올바르게 처리합니다.

2. **색상 평균 계산**:
   - 블록 내 모든 픽셀의 R, G, B 값을 누적합니다.
   - 블록 내 픽셀 수를 세어 평균을 계산할 때 사용합니다.
   - 각 색상 채널마다 정수 나눗셈(`//`)을 사용하여 평균을 계산합니다.

3. **평균 색상 적용**:
   - 계산된 평균 색상을 블록 내 모든 픽셀에 적용합니다.
   - 중첩된 루프를 사용하여 블록 내의 모든 픽셀을 처리합니다.

4. **정수 나눗셈**:
   - 문제 요구사항에 따라 평균 계산 시 정수 나눗셈(`//`)을 사용합니다.
   - 이로 인해 소수점 이하 값은 버려집니다.
