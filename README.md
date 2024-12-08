// 로컬 스토리지에 데이터를 저장하는 함수
function setItemWithExpiration(key, value) {
  const timestamp = new Date().getTime(); // 현재 시간의 타임스탬프 (밀리초)
  localStorage.setItem(key, timestamp);
}

// 로컬 스토리지에서 데이터를 가져오는 함수
function getItemWithExpiration(key) {
	const timestamp = localStorage.getItem(key);
  
  if (!timestamp) {
    return null;
  }

  const currentTime = new Date().getTime();
  const expirationTime = 24 * 60 * 60 * 1000; // 24시간을 밀리초로 변환
  
  if (currentTime - timestamp > expirationTime) {
    // 24시간이 경과하면 로컬 스토리지에서 해당 key 삭제
    localStorage.removeItem(key);
    console.log(key + "값이 만료되어 삭제되었습니다.");

    return null;
  } else {
    // 24시간 이내라면 저장된 타임스탬프 출력
    console.log(key + " (이미 있음) 타임스탬프: " + timestamp);

    return timestamp; // 24시간이 지나지 않았으면 저장된 값 반환
  }
}

// 데이터 저장 (key)
// setItemWithExpiration("test");

// 데이터 가져오기
const todayVideo = getItemWithExpiration("test");
if (todayVideo) {
  console.log("key:", todayVideo);
} else {
  console.log("만료되거나 없음");
  setItemWithExpiration("test");
}
