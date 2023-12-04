# labs1-4
___Лабораторна робота № 1: «Ініціалізація середовища OpenGL ES»___

Хід роботи

1.	В новому проекті виконайте ініціалізацію графічного середовища
2.  Визначте графічні примітиви: трикутник 

![](https://i.ibb.co/p1bmZLQ/Screenshot-18.png)

___Лабораторна робота № 2: «Визначення графічних об’єктів»___

Хід роботи

Визначення типу проекції та позиції камери, яка задає точку зору

Результат до:

![](https://i.ibb.co/2s069nh/Screenshot-19.png)


__Код__

private val vPMatrix = FloatArray(16)

private val projectionMatrix = FloatArray(16)

private val viewMatrix = FloatArray(16)


override fun onSurfaceChanged(unused: GL10, width: Int, height: Int) {
    GLES20.glViewport(0, 0, width, height)

    val ratio: Float = width.toFloat() / height.toFloat()

    // this projection matrix is applied to object coordinates
    // in the onDrawFrame() method
    Matrix.frustumM(projectionMatrix, 0, -ratio, ratio, -2f, 4f, 4f, 2f)
}

Результат:

![](https://i.ibb.co/QrgPZ5x/Screenshot-20.png)

___Лабораторна робота № 3: «Обертання зображення»___
Хід роботи

Результат до:

![](https://i.ibb.co/JBpLVy9/Screenshot-21.png)

__Код:__

override fun onDrawFrame(gl: GL10) {

    val resultMatrix = FloatArray(16)

    // Очистка фона
    GLES20.glClear(GLES20.GL_COLOR_BUFFER_BIT)

    // Установка позиции камеры (матрица вида)
    Matrix.setLookAtM(cameraViewMatrix, 0, 0f, 0f, 3f, 0f, 0f, 0f, 0f, 1.0f, 0.0f)

    // Расчёт преобразований проекции и вида
    Matrix.multiplyMM(viewProjectionMatrix, 0, projectionMatrix, 0, cameraViewMatrix, 0)

    // Создание матрицы вращения для треугольника
    Matrix.setRotateM(rotationMatrix, 0, angle, 0f, 0f, -1.0f)

    // Комбинирование матрицы вращения с проекцией и видом камеры
    // Обратите внимание, что фактор vPMatrix *должен быть первым* для правильного
    // выполнения произведения матрицы умножения.
    Matrix.multiplyMM(resultMatrix, 0, viewProjectionMatrix, 0, rotationMatrix, 0)

    // Отрисовка треугольника
    mTriangle.draw(resultMatrix)
}

Результат:

![](https://i.ibb.co/kDsNY8g/Screenshot-22.png)

___Лабораторна робота № 4: «Реагування на дотик»___

Хід роботи

___Код___

override fun onTouchEvent(event: MotionEvent): Boolean {
    // MotionEvent reports input details from the touch screen
    // and other input controls. In this case, you are only
    // interested in events where the touch position changed.

    val currentX: Float = event.x
    val currentY: Float = event.y

    when (event.action) {
        MotionEvent.ACTION_MOVE -> {

            var deltaX: Float = currentX - previousX
            var deltaY: Float = currentY - previousY

            // reverse direction of rotation above the mid-line
            if (currentY > height / 2) {
                deltaX *= -1
            }

            // reverse direction of rotation to the left of the mid-line
            if (currentX < width / 2) {
                deltaY *= -1
            }

            renderer.angle += (deltaX + deltaY) * TOUCH_SCALE_FACTOR
            requestRender()
        }
    }

    previousX = currentX
    previousY = currentY
    return true
}

Реузльтат:

![](https://i.ibb.co/C5Tmj5D/Screenshot-23.png)












