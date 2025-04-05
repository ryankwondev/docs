## Task 1: Image Strip Removal

### 라이브러리 임포트 및 함수 정의

```python
from cs1media import *

def remove_image_section(input_image, cut_start, cut_end, direction):
```

* `cs1media` 라이브러리의 모든 함수와 클래스를 가져옵니다.
* `remove_image_section` 함수는 이미지에서 특정 부분을 제거하는 기능을 구현합니다.
* 이 함수는 4개의 매개변수를 받습니다:
  * `input_image`: 원본 이미지 객체
  * `cut_start`: 제거할 영역의 시작 위치
  * `cut_end`: 제거할 영역의 끝 위치
  * `direction`: 제거할 방향 ("vertical" 또는 "horizontal")

### 이미지 크기 가져오기

```python
    width, height = input_image.size()
```

* `input_image.size()` 메서드를 호출하여 원본 이미지의 너비와 높이를 가져옵니다.
* 이 값들은 새 이미지의 크기를 계산하고 픽셀 위치를 조정하는 데 사용됩니다.

### 세로 방향 처리

```python
    if direction == "vertical":
        # Calculate new width after removing the vertical strip
        cut_width = cut_end - cut_start + 1
        new_width = width - cut_width
        new_height = height
        
        # Create new image with modified dimensions
        new_image = create_picture(new_width, new_height)
```

* `direction`이 "vertical"인 경우, 세로 방향으로 픽셀 열을 제거합니다.
* `cut_width`: 제거할 영역의 너비를 계산합니다. `cut_end - cut_start + 1`을 사용하는 이유는 `cut_end`를 포함해야 하기 때문입니다.
* `new_width`: 새 이미지의 너비는 원본 너비에서 제거된 너비를 뺀 값입니다.
* `new_height`: 높이는 변하지 않습니다.
* `create_picture` 함수를 호출하여 새로운 크기의 이미지를 생성합니다.

### 제거 전 영역 복사 (세로)

```python
        # Copy pixels before the cut
        for y in range(height):
            for x in range(cut_start):
                new_image.set(x, y, input_image.get(x, y))
```

* 이중 루프를 사용하여 제거할 영역 이전의 모든 픽셀을 복사합니다.
* 외부 루프 `for y in range(height)`: 이미지의 모든 행을 순회합니다.
* 내부 루프 `for x in range(cut_start)`: `cut_start` 이전의 모든 열을 순회합니다.
* `input_image.get(x, y)`로 원본 이미지에서 색상을 가져오고, `new_image.set(x, y, ...)`로 새 이미지에 동일한 위치에 설정합니다.

### 제거 후 영역 복사 (세로)

```python
        # Copy pixels after the cut (shifting left)
        for y in range(height):
            for x in range(cut_end + 1, width):
                new_x = x - cut_width
                new_image.set(new_x, y, input_image.get(x, y))
```

* 제거할 영역 이후의 모든 픽셀을 복사하되, 왼쪽으로 이동시킵니다.
* 외부 루프 `for y in range(height)`: 이미지의 모든 행을 순회합니다.
* 내부 루프 `for x in range(cut_end + 1, width)`: `cut_end` 이후부터 이미지 끝까지의 모든 열을 순회합니다.
* `new_x = x - cut_width`: 픽셀을 왼쪽으로 이동시키기 위해 `cut_width`만큼 x 좌표를 감소시킵니다.
* 원본 위치 `(x, y)`의 색상을 새 위치 `(new_x, y)`에 설정합니다.

### 가로 방향 처리

```python
    else:  # direction == "horizontal"
        # Calculate new height after removing the horizontal strip
        cut_height = cut_end - cut_start + 1
        new_width = width
        new_height = height - cut_height
        
        # Create new image with modified dimensions
        new_image = create_picture(new_width, new_height)
```

* `direction`이 "horizontal"인 경우, 가로 방향으로 픽셀 행을 제거합니다.
* `cut_height`: 제거할 영역의 높이를 계산합니다.
* `new_width`: 너비는 변하지 않습니다.
* `new_height`: 새 이미지의 높이는 원본 높이에서 제거된 높이를 뺀 값입니다.
* 새로운 크기의 이미지를 생성합니다.

### 제거 전 영역 복사 (가로)

```python
        # Copy pixels before the cut
        for y in range(cut_start):
            for x in range(width):
                new_image.set(x, y, input_image.get(x, y))
```

* 제거할 영역 이전의 모든 픽셀을 복사합니다.
* 외부 루프 `for y in range(cut_start)`: `cut_start` 이전의 모든 행을 순회합니다.
* 내부 루프 `for x in range(width)`: 각 행의 모든 열을 순회합니다.
* 원본 위치 `(x, y)`의 색상을 새 이미지의 동일한 위치에 설정합니다.

### 제거 후 영역 복사 (가로)

```python
        # Copy pixels after the cut (shifting up)
        for y in range(cut_end + 1, height):
            for x in range(width):
                new_y = y - cut_height
                new_image.set(x, new_y, input_image.get(x, y))
```

* 제거할 영역 이후의 모든 픽셀을 복사하되, 위로 이동시킵니다.
* 외부 루프 `for y in range(cut_end + 1, height)`: `cut_end` 이후부터 이미지 끝까지의 모든 행을 순회합니다.
* 내부 루프 `for x in range(width)`: 각 행의 모든 열을 순회합니다.
* `new_y = y - cut_height`: 픽셀을 위로 이동시키기 위해 `cut_height`만큼 y 좌표를 감소시킵니다.
* 원본 위치 `(x, y)`의 색상을 새 위치 `(x, new_y)`에 설정합니다.

### 결과 반환

```python
    return new_image
```

* 처리된 새 이미지를 함수의 결과로 반환합니다.

### 메인 함수

```python
def main():
    # Example test image
    input_image = load_picture('./sample_images/01.png')
    # input_image = load_picture('./sample_images/02.png')
    # input_image = load_picture('./sample_images/03.png')
    # input_image = load_picture('./sample_images/04.png')
    # input_image = load_picture('./sample_images/05.png')
    
    # Example values (you may test different values)
    cut_start = 100
    cut_end = 200
    direction = "vertical"  # or "horizontal"
    
    output_image = remove_image_section(input_image, cut_start, cut_end, direction)
    output_image.show()


if __name__ == '__main__':
    main()
```

* `main` 함수는 프로그램의 실행 시작점입니다.
* `load_picture` 함수를 사용하여 샘플 이미지를 로드합니다. 현재는 하나의 이미지만 로드하고 나머지는 주석 처리되어 있습니다.
* 테스트 매개변수를 설정합니다: `cut_start=100`, `cut_end=200`, `direction="vertical"`
* `remove_image_section` 함수를 호출하여 이미지를 처리하고 결과를 `output_image`에 저장합니다.
* `output_image.show()` 메서드를 호출하여 결과 이미지를 화면에 표시합니다.
* `if __name__ == '__main__':` 구문은 이 파일이 직접 실행될 때만 `main` 함수를 호출하도록 합니다.

## Task 2: Basic Image Transformation

### 라이브러리 임포트 및 함수 정의 (Task 2.1: Reflection)

```python
from cs1media import *
from module import combine_three_images

def reflect_image(input_image, direction):
```

* `cs1media` 라이브러리의 모든 함수와 클래스를 가져옵니다.
* `module`에서 `combine_three_images` 함수를 가져옵니다. 이 함수는 세 개의 이미지를 하나로 결합하는 유틸리티 함수입니다.
* `reflect_image` 함수는 이미지를 수평 또는 수직으로 뒤집는 기능을 구현합니다.
* 이 함수는 2개의 매개변수를 받습니다:
  * `input_image`: 원본 이미지 객체
  * `direction`: 반사 방향 ("horizontal" 또는 "vertical")

### 이미지 크기 및 초기화

```python
    width, height = input_image.size()
    output_image = create_picture(width, height)
```

* 원본 이미지의 너비와 높이를 가져옵니다.
* 동일한 크기의 새 이미지를 생성합니다.

### 수평 반사 (좌우 반전)

```python
    if direction == "horizontal":
        for y in range(height):
            for x in range(width):
                output_image.set(width - 1 - x, y, input_image.get(x, y))
```

* `direction`이 "horizontal"인 경우, 이미지를 좌우로 뒤집습니다.
* 이중 루프를 사용하여 모든 픽셀을 순회합니다.
* `width - 1 - x`: 좌우 반전을 위해 x 좌표를 반대로 뒤집습니다.
* 원본 위치 `(x, y)`의 색상을 새 위치 `(width - 1 - x, y)`에 설정합니다.

### 수직 반사 (상하 반전)

```python
    else:  # direction == "vertical"
        for y in range(height):
            for x in range(width):
                output_image.set(x, height - 1 - y, input_image.get(x, y))
```

* `direction`이 "vertical"인 경우, 이미지를 상하로 뒤집습니다.
* 이중 루프를 사용하여 모든 픽셀을 순회합니다.
* `height - 1 - y`: 상하 반전을 위해 y 좌표를 반대로 뒤집습니다.
* 원본 위치 `(x, y)`의 색상을 새 위치 `(x, height - 1 - y)`에 설정합니다.

### 결과 반환

```python
    return output_image
```

* 처리된 새 이미지를 함수의 결과로 반환합니다.

### 함수 정의 (Task 2.2: Rotation)

```python
def rotate_image(input_image, angle):
```

* `rotate_image` 함수는 이미지를 지정된 각도로 회전시키는 기능을 구현합니다.
* 이 함수는 2개의 매개변수를 받습니다:
  * `input_image`: 원본 이미지 객체
  * `angle`: 회전 각도 (90도의 배수)

### 이미지 크기 및 각도 정규화

```python
    width, height = input_image.size()
    
    # Normalize angle to be between 0 and 270
    angle = angle % 360
    if angle < 0:
        angle = (angle + 360) % 360
```

* 원본 이미지의 너비와 높이를 가져옵니다.
* 각도를 0~359 범위로 정규화합니다: `angle % 360`
* 음수 각도는 양수로 변환합니다: `(angle + 360) % 360`
* 이렇게 하면 어떤 각도든 0, 90, 180, 270 중 하나로 변환됩니다.

### 0도 회전 (변환 없음)

```python
    if angle == 0:
        # No rotation
        return input_image
```

* 각도가 0도인 경우, 회전이 필요 없으므로 원본 이미지를 그대로 반환합니다.

### 90도 회전

```python
    elif angle == 90:
        # 90 degrees clockwise
        output_image = create_picture(height, width)
        for y in range(height):
            for x in range(width):
                output_image.set(height - 1 - y, x, input_image.get(x, y))
```

* 각도가 90도인 경우, 이미지를 시계 방향으로 90도 회전시킵니다.
* 회전 후 이미지의 크기가 바뀌므로 `create_picture(height, width)`와 같이 너비와 높이를 바꿔서 새 이미지를 생성합니다.
* 이중 루프를 사용하여 모든 픽셀을 순회합니다.
* 좌표 변환: `(x, y) → (height - 1 - y, x)`
  * 픽셀이 90도 회전하면 y 좌표가 새 이미지의 x 좌표가 되고 (반전되어), x 좌표가 새 이미지의 y 좌표가 됩니다.

### 180도 회전

```python
    elif angle == 180:
        # 180 degrees
        output_image = create_picture(width, height)
        for y in range(height):
            for x in range(width):
                output_image.set(width - 1 - x, height - 1 - y, input_image.get(x, y))
```

* 각도가 180도인 경우, 이미지를 180도 회전시킵니다.
* 회전 후 이미지의 크기는 변하지 않으므로 원본과 동일한 크기의 새 이미지를 생성합니다.
* 이중 루프를 사용하여 모든 픽셀을 순회합니다.
* 좌표 변환: `(x, y) → (width - 1 - x, height - 1 - y)`
  * 180도 회전은 x와 y 좌표 모두 반전시키는 것과 같습니다.

### 270도 회전

```python
    else:  # angle == 270
        # 270 degrees clockwise (90 degrees counterclockwise)
        output_image = create_picture(height, width)
        for y in range(height):
            for x in range(width):
                output_image.set(y, width - 1 - x, input_image.get(x, y))
```

* 각도가 270도인 경우, 이미지를 시계 방향으로 270도 회전시킵니다 (반시계 방향으로 90도와 동일).
* 90도와 마찬가지로 이미지의 크기가 바뀌므로 `create_picture(height, width)`와 같이 새 이미지를 생성합니다.
* 이중 루프를 사용하여 모든 픽셀을 순회합니다.
* 좌표 변환: `(x, y) → (y, width - 1 - x)`
  * 270도 회전은 x 좌표가 새 이미지의 y 좌표가 되고 (반전되어), y 좌표가 새 이미지의 x 좌표가 됩니다.

### 결과 반환

```python
    return output_image
```

* 처리된 새 이미지를 함수의 결과로 반환합니다.

### 함수 정의 (Task 2.3: Translation)

```python
def translate_image(input_image, shift_x, shift_y):
```

* `translate_image` 함수는 이미지를 지정된 픽셀 수만큼 이동시키는 기능을 구현합니다.
* 이 함수는 3개의 매개변수를 받습니다:
  * `input_image`: 원본 이미지 객체
  * `shift_x`: x축 이동량 (양수는 오른쪽, 음수는 왼쪽)
  * `shift_y`: y축 이동량 (양수는 아래쪽, 음수는 위쪽)

### 이미지 크기 및 이동값 최적화

```python
    width, height = input_image.size()
    output_image = create_picture(width, height)
    
    # Ensure shifts are within image dimensions (positive or negative)
    shift_x = shift_x % width
    shift_y = shift_y % height
```

* 원본 이미지의 너비와 높이를 가져옵니다.
* 동일한 크기의 새 이미지를 생성합니다.
* 이동값을 이미지 크기로 모듈로 연산하여 최적화합니다.
  * 이렇게 하면 이미지 크기보다 큰 이동도 효율적으로 처리할 수 있습니다.
  * 예를 들어, 너비가 100인 이미지에서 `shift_x = 120`은 `shift_x = 20`과 동일합니다.

### 이미지 이동 및 Wrap-around 처리

```python
    # Process each destination pixel in the output image
    for new_y in range(height):
        for new_x in range(width):
            # Calculate the source pixel position (reverse mapping)
            orig_x = (new_x - shift_x) % width
            orig_y = (new_y - shift_y) % height
            
            # Copy the color from the source position to the destination
            output_image.set(new_x, new_y, input_image.get(orig_x, orig_y))
```

* 이중 루프를 사용하여 새 이미지의 모든 픽셀을 순회합니다.
* 역매핑 방식을 사용하여 각 대상 픽셀 위치 `(new_x, new_y)`에 대해 원본 이미지의 어떤 픽셀이 해당 위치로 이동하는지 계산합니다.
* `orig_x = (new_x - shift_x) % width`: x 좌표 역매핑
* `orig_y = (new_y - shift_y) % height`: y 좌표 역매핑
* 모듈로 연산(`%`)을 사용하여 이미지 경계를 벗어나는 값을 반대편으로 감싸줍니다 (wrap-around).
* 원본 위치 `(orig_x, orig_y)`의 색상을 새 위치 `(new_x, new_y)`에 설정합니다.

### 결과 반환

```python
    return output_image
```

* 처리된 새 이미지를 함수의 결과로 반환합니다.

### 메인 함수

```python
def main():
    # Example test image
    input_image = load_picture('./sample_images/01.png')
    # input_image = load_picture('./sample_images/02.png')
    # input_image = load_picture('./sample_images/03.png')
    # input_image = load_picture('./sample_images/04.png')
    # input_image = load_picture('./sample_images/05.png')

    # Task 2.1: Reflection
    direction = "horizontal"  # Or "vertical"
    reflected_image = reflect_image(input_image, direction)

    # Task 2.2: Rotation
    angle = 90  # Change to different angles for testing
    rotated_image = rotate_image(input_image, angle)
    
    # Task 2.3: Translation (Wrap-around)
    shift_x = 100
    shift_y = -50
    translated_image = translate_image(input_image, shift_x, shift_y)
    
    # Combine and show
    combined = combine_three_images(reflected_image, rotated_image, translated_image)
    combined.show()  # Show both images together in one view


if __name__ == '__main__':
    main()
```

* `main` 함수는 프로그램의 실행 시작점입니다.
* 샘플 이미지를 로드합니다.
* Task 2.1 (Reflection): `reflect_image` 함수를 호출하여 이미지를 수평으로 뒤집습니다.
* Task 2.2 (Rotation): `rotate_image` 함수를 호출하여 이미지를 90도 회전시킵니다.
* Task 2.3 (Translation): `translate_image` 함수를 호출하여 이미지를 x축으로 100픽셀, y축으로 -50픽셀 이동시킵니다.
* `combine_three_images` 함수를 호출하여 세 개의 결과 이미지를 하나로 결합합니다.
* `combined.show()` 메서드를 호출하여 결합된 이미지를 화면에 표시합니다.
* `if __name__ == '__main__':` 구문은 이 파일이 직접 실행될 때만 `main` 함수를 호출하도록 합니다.

## Task 3: Image Mosaic

### 라이브러리 임포트 및 함수 정의 (Task 3.1: Basic Mosaic)

```python
from cs1media import *
from module import combine_two_images

def mosaic_simple(input_image, block_size):
```

* `cs1media` 라이브러리의 모든 함수와 클래스를 가져옵니다.
* `module`에서 `combine_two_images` 함수를 가져옵니다. 이 함수는 두 개의 이미지를 하나로 결합하는 유틸리티 함수입니다.
* `mosaic_simple` 함수는 이미지를 블록으로 나누고 각 블록을 왼쪽 상단 픽셀의 색상으로 채우는 기능을 구현합니다.
* 이 함수는 2개의 매개변수를 받습니다:
  * `input_image`: 원본 이미지 객체
  * `block_size`: 모자이크 블록의 크기

### 이미지 크기 및 초기화

```python
    width, height = input_image.size()
    output_image = create_picture(width, height)
```

* 원본 이미지의 너비와 높이를 가져옵니다.
* 동일한 크기의 새 이미지를 생성합니다.

### 모자이크 블록 처리

```python
    # Process full blocks and handle leftovers
    for y_start in range(0, height, block_size):
        for x_start in range(0, width, block_size):
            # Calculate the actual block size (might be smaller at edges)
            actual_width = min(block_size, width - x_start)
            actual_height = min(block_size, height - y_start)
```

* 이중 루프를 사용하여 이미지를 블록 단위로 순회합니다.
* `range(0, height, block_size)` 및 `range(0, width, block_size)`를 사용하여 `block_size` 간격으로 이미지를 분할합니다.
* 각 블록의 시작점은 `(x_start, y_start)`입니다.
* 이미지 가장자리에서는 블록 크기가 `block_size`보다 작을 수 있으므로, 실제 블록 크기를 계산합니다.
* `actual_width = min(block_size, width - x_start)`: 블록의 실제 너비
* `actual_height = min(block_size, height - y_start)`: 블록의 실제 높이

### 블록 색상 설정 (단순 모자이크)

```python
            # Get the color from the top-left pixel of the block
            block_color = input_image.get(x_start, y_start)
            
            # Fill the entire block with this color
            for y_offset in range(actual_height):
                for x_offset in range(actual_width):
                    output_image.set(x_start + x_offset, y_start + y_offset, block_color)
```

* 블록의 왼쪽 상단 픽셀 `(x_start, y_start)`의 색상을 가져옵니다.
* 이중 루프를 사용하여 블록 내의 모든 픽셀을 순회합니다.
* 블록 내의 모든 픽셀을 왼쪽 상단 픽셀의 색상으로 설정합니다.
* `x_start + x_offset`, `y_start + y_offset`를 사용하여 블록 내의 각 픽셀 위치를 계산합니다.

### 결과 반환

```python
    return output_image
```

* 처리된 새 이미지를 함수의 결과로 반환합니다.

### 함수 정의 (Task 3.2: Average Mosaic)

```python
def mosaic_average(input_image, block_size):
```

* `mosaic_average` 함수는 이미지를 블록으로 나누고 각 블록을 블록 내 모든 픽셀의 평균 색상으로 채우는 기능을 구현합니다.
* 이 함수는 2개의 매개변수를 받습니다:
  * `input_image`: 원본 이미지 객체
  * `block_size`: 모자이크 블록의 크기

### 이미지 크기 및 초기화

```python
    width, height = input_image.size()
    output_image = create_picture(width, height)
```

* 원본 이미지의 너비와 높이를 가져옵니다.
* 동일한 크기의 새 이미지를 생성합니다.

### 모자이크 블록 처리

```python
    # Process full blocks and handle leftovers
    for y_start in range(0, height, block_size):
        for x_start in range(0, width, block_size):
            # Calculate the actual block size (might be smaller at edges)
            actual_width = min(block_size, width - x_start)
            actual_height = min(block_size, height - y_start)
```

* 단순 모자이크와 동일하게 이미지를 블록 단위로 순회합니다.
* 각 블록의 실제 크기를 계산하여 가장자리 블록도 올바르게 처리합니다.

### 블록 내 픽셀 색상 평균 계산

```python
            # Calculate average color for this block
            total_r, total_g, total_b = 0, 0, 0
            pixel_count = 0
            
            # Sum up all pixel values in the block
            for y_offset in range(actual_height):
                for x_offset in range(actual_width):
                    r, g, b = input_image.get(x_start + x_offset, y_start + y_offset)
                    total_r += r
                    total_g += g
                    total_b += b
                    pixel_count += 1
```

* 평균 색상을 계산하기 위해 블록 내 모든 픽셀의 RGB 값을 누적합니다.
* `total_r`, `total_g`, `total_b`: 블록 내 모든 픽셀의 R, G, B 값 합계를 저장할 변수를 초기화합니다.
* `pixel_count`: 블록 내 픽셀 수를 세는 변수를 초기화합니다.
* 이중 루프를 사용하여 블록 내의 모든 픽셀을 순회합니다.
* `input_image.get(x_start + x_offset, y_start + y_offset)`로 픽셀의 RGB 색상 값을 가져옵니다.
* 각 색상 채널별로 합계에 더하고, 픽셀 카운터를 증가시킵니다.

### 평균 색상 계산 및 적용

```python
            # Calculate average (using integer division)
            avg_r = total_r // pixel_count
            avg_g = total_g // pixel_count
            avg_b = total_b // pixel_count
            avg_color = (avg_r, avg_g, avg_b)
            
            # Fill the entire block with the average color
            for y_offset in range(actual_height):
                for x_offset in range(actual_width):
                    output_image.set(x_start + x_offset, y_start + y_offset, avg_color)
```

* 블록 내 모든 픽셀의 평균 색상을 계산합니다.
* `//` 연산자를 사용하여 정수 나눗셈을 수행합니다 (문제 요구사항에 따름).
* `avg_color`: 계산된 평균 색상을 RGB 튜플로 저장합니다.
* 이중 루프를 사용하여 블록 내의 모든 픽셀을 다시 순회합니다.
* 블록 내의 모든 픽셀을 계산된 평균 색상으로 설정합니다.

### 결과 반환

```python
    return output_image
```

* 처리된 새 이미지를 함수의 결과로 반환합니다.

### 메인 함수

```python
def main():
    # Example test image
    input_image = load_picture('./sample_images/01.png')
    # input_image = load_picture('./sample_images/02.png')
    # input_image = load_picture('./sample_images/03.png')
    # input_image = load_picture('./sample_images/04.png')
    # input_image = load_picture('./sample_images/05.png')

    # Example block size
    block_size = 20  # Example block size

    # Task 3.1: Basic Mosaic
    simple_mosaic_image = mosaic_simple(input_image, block_size)

    # Task 3.2: Average Mosaic
    average_mosaic_image = mosaic_average(input_image, block_size)

    # Combine and show
    combined = combine_two_images(simple_mosaic_image, average_mosaic_image)
    combined.show()  # Show both images together in one view


if __name__ == '__main__':
    main()
```

* `main` 함수는 프로그램의 실행 시작점입니다.
* 샘플 이미지를 로드합니다.
* 모자이크 블록 크기를 20으로 설정합니다.
* Task 3.1 (Basic Mosaic): `mosaic_simple` 함수를 호출하여 단순 모자이크 이미지를 생성합니다.
* Task 3.2 (Average Mosaic): `mosaic_average` 함수를 호출하여 평균 모자이크 이미지를 생성합니다.
* `combine_two_images` 함수를 호출하여 두 개의 결과 이미지를 하나로 결합합니다.
* `combined.show()` 메서드를 호출하여 결합된 이미지를 화면에 표시합니다.
* `if __name__ == '__main__':` 구문은 이 파일이 직접 실행될 때만 `main` 함수를 호출하도록 합니다.
