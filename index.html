import React, { useState, useEffect, useCallback } from 'react';

// Firebase 및 인증 관련 함수는 사용되지 않으므로 제거합니다.
// Firestore는 데이터 저장이 필요하지 않은 단일 비전 작업이므로 생략합니다.

// API 키 및 URL 설정
const apiKey = ""; 
const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

/**
 * 사용자에게 이미지를 업로드받고, Gemini Vision API를 사용하여
 * 자동으로 음식의 종류와 설명을 인식하여 표시하는 단일 React 컴포넌트입니다.
 */
const App = () => {
    // 앱 상태 관리
    const [selectedImage, setSelectedImage] = useState(null); // base64 이미지 데이터
    const [recognitionResult, setRecognitionResult] = useState(''); // 인식 결과 텍스트
    const [isLoading, setIsLoading] = useState(false); // 로딩 상태
    const [error, setError] = useState(null); // 에러 메시지
    
    // 이 컴포넌트에서는 Tailwind CSS를 사용한다고 가정합니다.

    // 파일을 base64로 변환하는 헬퍼 함수
    const fileToBase64 = (file) => {
        return new Promise((resolve, reject) => {
            const reader = new FileReader();
            reader.readAsDataURL(file);
            reader.onload = () => resolve(reader.result);
            reader.onerror = error => reject(error);
        });
    };

    // 이미지 파일 선택 핸들러
    const handleImageChange = async (event) => {
        const file = event.target.files[0];
        if (file) {
            setRecognitionResult('');
            setError(null);
            
            // 파일 크기 제한 (예: 5MB) - Gemini API는 큰 파일을 처리할 수 있지만, 
            // 프론트엔드 성능을 위해 적절한 크기를 유지하는 것이 좋습니다.
            const maxSizeMB = 5;
            if (file.size > maxSizeMB * 1024 * 1024) {
                setError(`이미지 파일은 ${maxSizeMB}MB를 초과할 수 없습니다.`);
                setSelectedImage(null);
                return;
            }

            try {
                const base64DataUrl = await fileToBase64(file);
                // mimeType과 base64 데이터만 추출
                const [mimeTypePart, dataPart] = base64DataUrl.split(',');
                const mimeTypeMatch = mimeTypePart.match(/:(.*?);/);
                
                if (mimeTypeMatch && dataPart) {
                    setSelectedImage({
                        data: dataPart,
                        mimeType: mimeTypeMatch[1]
                    });
                } else {
                    throw new Error("파일 형식을 파싱할 수 없습니다.");
                }
            } catch (err) {
                setError("이미지 변환 중 오류가 발생했습니다.");
                setSelectedImage(null);
            }
        }
    };

    // 음식 인식 및 설명 요청 함수 (API 호출)
    const identifyFood = useCallback(async () => {
        if (!selectedImage) {
            setError("먼저 이미지를 업로드해주세요.");
            return;
        }

        setIsLoading(true);
        setRecognitionResult('');
        setError(null);

        // Exponential Backoff 로직
        const maxRetries = 3;
        let attempt = 0;
        
        const systemPrompt = "당신은 전문적인 미식 AI 비서입니다. 당신의 임무는 이미지에 보이는 음식을 정확하게 식별하고, 해당 요리에 대한 간결하고 매력적인 설명을 한국어로 제공하는 것입니다. 가능한 주재료와 일반적인 서빙 방식에 대해 언급해 주세요. 이미지를 분석했다는 사실을 언급하지 마세요. 마치 눈앞에 음식을 보고 말하는 것처럼 자신감 있는 어투로 정보를 제시하세요.";
        const userQuery = "이 음식은 무엇인가요? 식별하고 설명해 주세요.";

        const payload = {
            contents: [
                {
                    role: "user",
                    parts: [
                        { text: userQuery },
                        {
                            inlineData: {
                                mimeType: selectedImage.mimeType,
                                data: selectedImage.data
                            }
                        }
                    ]
                }
            ],
            systemInstruction: {
                parts: [{ text: systemPrompt }]
            },
        };

        while (attempt < maxRetries) {
            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    // 429 Too Many Requests에 대해서만 재시도
                    if (response.status === 429 && attempt < maxRetries - 1) {
                        const delay = Math.pow(2, attempt) * 1000; // 1s, 2s, 4s
                        await new Promise(resolve => setTimeout(resolve, delay));
                        attempt++;
                        continue;
                    }
                    throw new Error(`API 요청 실패: ${response.statusText}`);
                }

                const result = await response.json();
                const text = result.candidates?.[0]?.content?.parts?.[0]?.text;

                if (text) {
                    setRecognitionResult(text);
                } else {
                    throw new Error("API에서 유효한 응답 텍스트를 받지 못했습니다.");
                }

                break; // 성공 시 루프 종료

            } catch (err) {
                if (attempt === maxRetries - 1) {
                    console.error("최종 API 호출 오류:", err);
                    setError("음식 인식에 실패했습니다. 이미지가 명확한지 확인하고 다시 시도해 주세요.");
                }
                attempt++;
            }
        }
        setIsLoading(false);
    }, [selectedImage]);


    // UI 렌더링
    return (
        <div className="min-h-screen bg-gray-50 flex flex-col items-center p-4 sm:p-8 font-['Inter']">
            {/* 불필요한 Warning을 유발하는 <style jsx global> 태그를 제거했습니다. 
                Tailwind CSS는 <script> 태그를 통해 로드하며, 폰트 클래스는 최상위 div에 적용되어 있습니다. */}
            <script src="https://cdn.tailwindcss.com"></script>

            {/* 헤더 */}
            <header className="w-full max-w-4xl text-center mb-8">
                <h1 className="text-4xl font-extrabold text-indigo-700 mb-2">
                    AI 푸드 마스터
                </h1>
                <p className="text-gray-600 text-lg">
                    이미지를 업로드하면, AI가 자동으로 음식을 인식하고 설명해 드립니다.
                </p>
                <div className="w-24 h-1 bg-indigo-500 mx-auto mt-3 rounded-full"></div>
            </header>

            {/* 메인 컨테이너 */}
            <main className="w-full max-w-4xl bg-white p-6 sm:p-10 rounded-2xl shadow-xl border border-gray-100">

                {/* 이미지 업로드 영역 */}
                <div className="mb-8">
                    <label 
                        htmlFor="file-upload" 
                        className="block text-sm font-medium text-gray-700 mb-3"
                    >
                        1. 음식 사진을 선택하세요 (PNG 또는 JPG)
                    </label>
                    <input
                        id="file-upload"
                        type="file"
                        accept="image/png, image/jpeg"
                        onChange={handleImageChange}
                        className="block w-full text-sm text-gray-500
                            file:mr-4 file:py-2 file:px-4
                            file:rounded-full file:border-0
                            file:text-sm file:font-semibold
                            file:bg-indigo-50 file:text-indigo-700
                            hover:file:bg-indigo-100 cursor-pointer"
                        disabled={isLoading}
                    />
                </div>

                <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    {/* 이미지 미리보기 */}
                    <div className="relative p-4 border-2 border-dashed border-gray-300 rounded-xl min-h-[300px] flex flex-col justify-center items-center bg-gray-50">
                        <h2 className="text-xl font-semibold text-gray-700 mb-4">
                            업로드된 이미지
                        </h2>
                        {selectedImage ? (
                            <img
                                src={`data:${selectedImage.mimeType};base64,${selectedImage.data}`}
                                alt="업로드된 음식 이미지"
                                className="max-h-96 w-full object-contain rounded-lg shadow-md"
                            />
                        ) : (
                            <p className="text-gray-500 italic p-10">
                                여기에 선택한 이미지가 표시됩니다.
                            </p>
                        )}
                    </div>

                    {/* 인식 결과 영역 */}
                    <div className="p-4 bg-indigo-50 rounded-xl shadow-inner min-h-[300px] flex flex-col">
                        <h2 className="text-xl font-semibold text-indigo-700 mb-4 flex items-center">
                            2. AI 인식 결과
                        </h2>
                        
                        {/* 인식 버튼 */}
                        <button
                            onClick={identifyFood}
                            disabled={!selectedImage || isLoading}
                            className={`w-full py-3 px-6 mb-4 text-white font-bold rounded-full transition duration-200 shadow-lg 
                                ${!selectedImage || isLoading 
                                    ? 'bg-gray-400 cursor-not-allowed' 
                                    : 'bg-indigo-600 hover:bg-indigo-700 active:bg-indigo-800 transform hover:scale-[1.01]'}
                            `}
                        >
                            {isLoading ? '인식 중...' : '💡 음식 자동 인식 시작'}
                        </button>
                        
                        {/* 로딩/에러/결과 메시지 */}
                        {error && (
                            <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-lg relative mb-4" role="alert">
                                <span className="block sm:inline">{error}</span>
                            </div>
                        )}

                        {isLoading && (
                            <div className="flex justify-center items-center h-full">
                                <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-indigo-600"></div>
                                <p className="ml-4 text-indigo-600 font-medium">AI가 메뉴를 신중하게 분석 중입니다...</p>
                            </div>
                        )}

                        {!isLoading && recognitionResult && (
                            <div className="flex-1 bg-white p-6 rounded-lg shadow-md overflow-y-auto">
                                <div className="text-gray-800 whitespace-pre-wrap">
                                    <h3 className="text-2xl font-bold text-green-600 mb-3">식별 성공!</h3>
                                    {recognitionResult}
                                </div>
                            </div>
                        )}

                        {!selectedImage && !isLoading && !error && !recognitionResult && (
                             <p className="text-gray-500 italic text-center py-10">
                                이미지를 업로드하고 버튼을 눌러주세요.
                            </p>
                        )}
                    </div>
                </div>
            </main>
        </div>
    );
};

export default App;
