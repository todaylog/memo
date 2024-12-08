# memo

// 로컬 스토리지에 데이터를 저장하는 함수
function setItemWithExpiration(key, value) {
  const data = {
    value: value,
    timestamp: new Date().getTime() // 현재 시간(밀리초 단위)
  };
  localStorage.setItem(key, JSON.stringify(data));
}

// 로컬 스토리지에서 데이터를 가져오는 함수
function getItemWithExpiration(key) {
  const data = JSON.parse(localStorage.getItem(key));
  
  if (!data) {
    return null; // 데이터가 없으면 null 반환
  }

  const currentTime = new Date().getTime();
  const expirationTime = 24 * 60 * 60 * 1000; // 24시간 = 24 * 60 * 60 * 1000 밀리초
  
  if (currentTime - data.timestamp > expirationTime) {
    localStorage.removeItem(key); // 24시간이 지나면 데이터 삭제
    return null; // 삭제 후 null 반환
  }

  return data.value; // 24시간이 지나지 않았으면 저장된 값 반환
}

// 사용 예시

// 데이터 저장 (예: 사용자 이름)
setItemWithExpiration("username", "JohnDoe");

// 데이터 가져오기
const username = getItemWithExpiration("username");
if (username) {
  console.log("저장된 사용자 이름:", username);
} else {
  console.log("저장된 사용자 이름이 없거나 만료되었습니다.");
}
