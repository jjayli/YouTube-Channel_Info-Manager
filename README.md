YouTube 채널 자동 관리 시트 (YouTube Channel Manager)

단순 복사/붙여넣기만으로 유튜브 채널 정보를 자동으로 표로 정리해주는 간단한 웹 애플리케이션입니다. 별도의 설치 없이 HTML 파일 하나만으로 동작하며, 여러 채널을 리스트업하고 관리하는 데 도움을 줍니다.


✨ 주요 기능 (Features)

자동 파싱: 유튜브 채널의 '정보' 탭에서 복사한 텍스트를 붙여넣으면 이메일, 구독자 수, 조회수 등을 자동으로 인식하여 표에 채워줍니다.

데이터 관리: 협조 메일 발송 여부를 체크하고, 각 채널에 대한 메모나 사용 가능 여부를 자유롭게 추가할 수 있습니다.

개별/전체 삭제: 각 채널 정보를 개별적으로 삭제하거나, 목록 전체를 한 번에 삭제할 수 있습니다.

CSV 내보내기: 정리된 모든 데이터를 CSV 파일로 한 번에 다운로드하여 구글 시트나 엑셀에서 영구적으로 관리할 수 있습니다.

설치 필요 없음: 단일 index.html 파일로 구성되어 인터넷만 연결되어 있다면 어떤 환경에서든 웹 브라우저로 바로 실행할 수 있습니다.


🚀 사용 방법 (How to Use)

**라이브 데모**에 접속하거나, 이 저장소에서 index.html 파일을 다운로드하여 웹 브라우저로 엽니다.

관리하고 싶은 유튜브 채널의 '정보' 탭으로 이동하여 페이지 전체 텍스트를 복사합니다.

웹 애플리케이션의 채널 정보 붙여넣기 칸에 복사한 내용을 붙여넣습니다.

채널 추가하기 버튼을 누르면 정보가 아래 표에 자동으로 추가됩니다.

작업이 끝나면 CSV로 내보내기 버튼을 눌러 데이터를 파일로 저장합니다.


💻 소스 코드 (Source Code)

이 도구는 단일 HTML 파일로 이루어져 있습니다. 아래 코드를 복사하여 index.html 파일로 저장하면 바로 사용할 수 있습니다.

<details>
<summary>▶︎ index.html 코드 보기</summary>

<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YouTube 채널 관리 시트</title>
    <script src="[https://cdn.tailwindcss.com](https://cdn.tailwindcss.com)"></script>
    <link href="[https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700&display=swap](https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700&display=swap)" rel="stylesheet">
    <style>
        body {
            font-family: 'Noto Sans KR', sans-serif;
        }
        .table-cell {
            padding: 12px 16px;
            border-bottom: 1px solid #e2e8f0;
            vertical-align: middle;
            font-size: 14px;
        }
        .table-header {
            background-color: #f1f5f9;
            color: #475569;
            font-weight: 700;
            text-align: left;
            font-size: 12px;
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }
        .editable-cell {
            cursor: text;
            min-width: 150px;
        }
        .editable-cell:focus {
            outline: 2px solid #6366f1;
            background-color: #eef2ff;
        }
        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .modal-overlay {
            transition: opacity 0.3s ease;
        }
        .btn-icon {
            width: 1.25rem;
            height: 1.25rem;
            margin-right: 0.5rem;
        }
    </style>
</head>
<body class="bg-slate-100 text-slate-800">

    <div class="container mx-auto p-4 sm:p-6 lg:p-8 max-w-7xl">
        <header class="mb-8 text-center">
            <h1 class="text-4xl font-bold text-slate-900">YouTube Channel Manager</h1>
            <p class="mt-2 text-slate-600">유튜브 채널 '정보'를 복사하여 붙여넣기만 하면 자동으로 표가 완성됩니다.</p>
        </header>

        <main>
            <!-- 입력 섹션 -->
            <div class="bg-white p-6 rounded-xl shadow-lg mb-8">
                <label for="channel-data" class="block text-sm font-medium text-slate-700 mb-2">채널 정보 붙여넣기:</label>
                <textarea id="channel-data" rows="8" class="w-full p-3 border border-slate-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition" placeholder="이곳에 유튜브 채널 정보 페이지의 텍스트를 붙여넣으세요..."></textarea>
                <div class="mt-4 flex flex-wrap gap-3">
                    <button onclick="parseAndAddChannel()" class="flex items-center justify-center flex-grow sm:flex-grow-0 bg-indigo-600 text-white font-bold py-2 px-5 rounded-lg hover:bg-indigo-700 transition duration-300 shadow-sm">
                        <svg class="btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6"></path></svg>
                        채널 추가
                    </button>
                    <button id="export-csv" onclick="exportToCSV()" class="flex items-center justify-center flex-grow sm:flex-grow-0 bg-emerald-600 text-white font-bold py-2 px-5 rounded-lg hover:bg-emerald-700 transition duration-300 shadow-sm hidden">
                        <svg class="btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path></svg>
                        CSV 내보내기
                    </button>
                    <button id="clear-all-btn" onclick="openClearAllModal()" class="flex items-center justify-center flex-grow sm:flex-grow-0 bg-rose-600 text-white font-bold py-2 px-5 rounded-lg hover:bg-rose-700 transition duration-300 shadow-sm hidden">
                       <svg class="btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                        전체 삭제
                    </button>
                </div>
            </div>

            <!-- 채널 목록 테이블 -->
            <div class="bg-white rounded-xl shadow-lg overflow-x-auto">
                <table class="min-w-full">
                    <thead class="table-header">
                        <tr>
                            <th class="table-cell">이메일</th>
                            <th class="table-cell">채널 URL</th>
                            <th class="table-cell">국가</th>
                            <th class="table-cell">가입일</th>
                            <th class="table-cell">구독자</th>
                            <th class="table-cell">동영상 수</th>
                            <th class="table-cell">총 조회수</th>
                            <th class="table-cell">사용가능 여부</th>
                            <th class="table-cell text-center">메일 발송</th>
                            <th class="table-cell">채널 내용</th>
                            <th class="table-cell">기타</th>
                            <th class="table-cell w-16 text-center">삭제</th>
                        </tr>
                    </thead>
                    <tbody id="channel-table-body" class="divide-y divide-slate-200">
                        <!-- 채널 데이터가 여기에 추가됩니다 -->
                    </tbody>
                </table>
                <p id="empty-table-message" class="p-8 text-center text-slate-500">아직 추가된 채널이 없습니다.</p>
            </div>
        </main>
    </div>
    
    <!-- 전체 삭제 확인 모달 -->
    <div id="clear-all-modal" class="fixed inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-full w-full z-50 modal-overlay hidden" style="opacity: 0;">
        <div class="relative top-20 mx-auto p-5 border w-96 shadow-lg rounded-md bg-white">
            <div class="mt-3 text-center">
                <div class="mx-auto flex items-center justify-center h-12 w-12 rounded-full bg-red-100">
                    <svg class="h-6 w-6 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path></svg>
                </div>
                <h3 class="text-lg leading-6 font-medium text-gray-900 mt-4">전체 삭제 확인</h3>
                <div class="mt-2 px-7 py-3">
                    <p class="text-sm text-gray-500">정말로 모든 채널 정보를 삭제하시겠습니까? 이 작업은 되돌릴 수 없습니다.</p>
                </div>
                <div class="items-center px-4 py-3 flex justify-center gap-4">
                    <button id="confirm-clear-btn" class="px-4 py-2 bg-red-500 text-white text-base font-medium rounded-md w-24 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500">
                        삭제
                    </button>
                    <button id="cancel-clear-btn" class="px-4 py-2 bg-gray-200 text-gray-800 text-base font-medium rounded-md w-24 hover:bg-gray-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500">
                        취소
                    </button>
                </div>
            </div>
        </div>
    </div>


    <script>
        function parseAndAddChannel() {
            const text = document.getElementById('channel-data').value.trim();
            if (!text) {
                alert('붙여넣을 데이터가 없습니다.');
                return;
            }

            const lines = text.split('\n').map(line => line.trim()).filter(line => line);
            
            const data = {
                email: '', url: '', country: '정보 없음', joinDate: '',
                subscribers: '', videos: '', views: ''
            };
            const unassigned = [];

            lines.forEach(line => {
                const emailRegex = /[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/;
                if (emailRegex.test(line) && !data.email) {
                    data.email = line.match(emailRegex)[0];
                } else if (line.includes('[youtube.com/](https://youtube.com/)')) {
                    const urlMatch = line.match(/https?:\/\/[^\s)]+/);
                    data.url = urlMatch ? urlMatch[0] : line;
                } else if (line.startsWith('가입일:')) {
                    data.joinDate = line.replace('가입일:', '').trim();
                } else if (line.startsWith('구독자')) {
                    data.subscribers = line.replace('구독자', '').trim();
                } else if (line.startsWith('동영상')) {
                    data.videos = line.replace('동영상', '').trim();
                } else if (line.startsWith('조회수')) {
                    data.views = line.replace('조회수', '').trim();
                } else {
                    unassigned.push(line);
                }
            });

            if (unassigned.length > 0) {
                const countryCandidate = unassigned.find(item => !item.includes('www.') && !item.includes('@'));
                if (countryCandidate) data.country = countryCandidate;
            }
            
            addChannelToTable(data);
            document.getElementById('channel-data').value = '';
        }

        function addChannelToTable(data) {
            const tableBody = document.getElementById('channel-table-body');
            const newRow = tableBody.insertRow(0);
            newRow.className = 'fade-in hover:bg-slate-50 transition-colors';
            const displayUrl = data.url.replace(/^https?:\/\/(www\.)?/, '');

            newRow.innerHTML = `
                <td class="table-cell">${createLink(data.email, `mailto:${data.email}`)}</td>
                <td class="table-cell">${createLink(displayUrl, data.url)}</td>
                <td class="table-cell">${data.country}</td>
                <td class="table-cell">${data.joinDate}</td>
                <td class="table-cell">${data.subscribers}</td>
                <td class="table-cell">${data.videos}</td>
                <td class="table-cell">${data.views}</td>
                <td class="table-cell editable-cell" contenteditable="true"></td>
                <td class="table-cell text-center"><input type="checkbox" class="h-5 w-5 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500"></td>
                <td class="table-cell editable-cell" contenteditable="true"></td>
                <td class="table-cell editable-cell" contenteditable="true"></td>
                <td class="table-cell text-center">
                    <button onclick="deleteRow(this)" class="text-slate-400 hover:text-rose-600 p-2 rounded-full transition-colors">
                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)"><path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm4 0a1 1 0 012 0v6a1 1 0 11-2 0V8z" clip-rule="evenodd"></path></svg>
                    </button>
                </td>
            `;
            updateTableVisibility();
        }
        
        function createLink(text, url) {
            if (!text) return '';
            const cleanText = text;
            const cleanUrl = url;
            return `<a href="${cleanUrl}" target="_blank" class="text-indigo-600 hover:underline hover:text-indigo-800 break-all">${cleanText}</a>`;
        }

        function deleteRow(buttonEl) {
            const row = buttonEl.closest('tr');
            row.style.transition = 'opacity 0.3s ease-out';
            row.style.opacity = '0';
            setTimeout(() => {
                row.remove();
                updateTableVisibility();
            }, 300);
        }

        function updateTableVisibility() {
            const tableBody = document.getElementById('channel-table-body');
            const isEmpty = tableBody.rows.length === 0;
            document.getElementById('empty-table-message').style.display = isEmpty ? 'block' : 'none';
            document.getElementById('export-csv').style.display = isEmpty ? 'none' : 'inline-flex';
            document.getElementById('clear-all-btn').style.display = isEmpty ? 'none' : 'inline-flex';
        }

        function openClearAllModal() {
            const modal = document.getElementById('clear-all-modal');
            modal.classList.remove('hidden');
            setTimeout(() => modal.style.opacity = 1, 10);
        }

        function closeClearAllModal() {
            const modal = document.getElementById('clear-all-modal');
            modal.style.opacity = 0;
            setTimeout(() => modal.classList.add('hidden'), 300);
        }
        
        document.getElementById('confirm-clear-btn').addEventListener('click', () => {
            document.getElementById('channel-table-body').innerHTML = '';
            updateTableVisibility();
            closeClearAllModal();
        });
        
        document.getElementById('cancel-clear-btn').addEventListener('click', closeClearAllModal);
        document.getElementById('clear-all-modal').addEventListener('click', (e) => {
            if (e.target.id === 'clear-all-modal') {
                closeClearAllModal();
            }
        });

        function exportToCSV() {
            const headers = ["이메일", "채널 URL", "국가", "가입일", "구독자", "동영상 수", "총 조회수", "사용가능 여부", "메일 발송", "채널 내용", "기타"];
            const table = document.querySelector("table");
            let csv = [headers.join(',')];

            const rows = table.querySelectorAll("tbody tr");

            rows.forEach(row => {
                const rowData = [];
                const cols = row.cells;
                
                rowData.push(`"${cols[0].innerText}"`);
                rowData.push(`"${cols[1].querySelector('a') ? cols[1].querySelector('a').href : cols[1].innerText}"`);
                rowData.push(`"${cols[2].innerText}"`);
                rowData.push(`"${cols[3].innerText}"`);
                rowData.push(`"${cols[4].innerText}"`);
                rowData.push(`"${cols[5].innerText}"`);
                rowData.push(`"${cols[6].innerText}"`);
                rowData.push(`"${cols[7].innerText.replace(/"/g, '""')}"`); // 사용가능 여부
                rowData.push(cols[8].querySelector('input[type="checkbox"]').checked ? '"발송 완료"' : '"미발송"');
                rowData.push(`"${cols[9].innerText.replace(/"/g, '""')}"`);
                rowData.push(`"${cols[10].innerText.replace(/"/g, '""')}"`);
                
                csv.push(rowData.join(','));
            });

            const csvContent = "data:text/csv;charset=utf-8,\uFEFF" + csv.join('\n');
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "youtube_channels.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }
    </script>

</body>
</html>
