# distanceCalibration

static void rootfunc(void * arg) {

	motor_init();  //모터 초기화
	encoder_init();  //모터 초기화
	glcd_init();  //glcd 초기화

	sensor_init(NXT_DIGITAL_SENSOR_SONA, 0, 0, 0);  //0번포트 센서 사용

	// calibration sensor : port0, light sensor
	calibSensor(0, MAX_SOUND_LEVEL, sound_value);  //Calibration

	task_sleep(3000); // 3s delay

	glcd_clear();  //glcd화면 초기화

	switch(get_level(sensor_get(0), MAX_SOUND_LEVEL, sound_value))  //센서값 레벨 나눔
	{  //케이스 구분을 위해 사용된 용어는 기존 라이브러리에서 그대로 가져옴
	case SOUND_QUIET:
		glcd_printf("MODE1 : %3d", sensor_get(0));  //모드1 출력
		break;

	case SOUND_TALKING:
		glcd_printf("MODE2 : %3d", sensor_get(0));  //모드2 출력
		break;

	case SOUND_SHOUT:
		glcd_printf("MODE3 : %3d", sensor_get(0));  //모드3 출력
		break;

	case SOUND_LOUD:
		glcd_printf("MODE4 : %3d", sensor_get(0));  //모드4 출력
		break;

	default:
		glcd_printf("UNDEFINE");
		break;
	}

	for(;;){

		if(sensor_get(0)<20){
			motor_set(0,200);}  //센서값 20보다 작으면 모터속도 200
		else if(sensor_get(0)<60){
			motor_set(0,500);}  //센서값 20~60이면 모터속도 500
		else{
			motor_set(0,800);}  //센서값 60보다 크면 모터속도 800

		task_sleep(30);		// 30ms delay
	}
}