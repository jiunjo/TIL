	1.	useLocation()은 React Router의 훅이다.
	2.	훅은 “그 자체로 함수”라 바로 속성에 접근할 수 없다.
	3.	그래서 반드시 실행해서 값을 받아야 한다.
	4.	즉, useLocation.pathname ❌ / const location = useLocation() ✔
	5.	실행 결과로 location 객체를 반환한다.
	6.	이 객체 안에 pathname, search, hash 같은 속성이 들어 있다.
	7.	그래서 location.pathname으로 현재 URL 경로를 읽을 수 있다.
	8.	네비게이션 메뉴에서 “현재 페이지 활성화 표시”할 때 자주 쓴다.
	9.	예: const isActive = (path) => location.pathname === path;
	10.	핵심: 훅 실행 → 객체 반영 → pathname 사용 가능 이 흐름.
