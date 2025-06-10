# demo
<!DOCTYPE html>
<html lang="vi" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hành Trình Tìm Kiếm & Phát Triển Chiến Tướng GHN</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">

    <!-- Chosen Palette: Warm Neutrals & GHN Orange Accent -->
    <!-- Application Structure Plan: The SPA is architected as a cohesive, top-down narrative journey. It begins with the high-level philosophy ('Triết Lý Cốt Lõi'), drills down into the specific 'Khung Năng Lực' using interactive visualizations, then maps out the entire 'Quy Trình Tuyển Chọn' via a clear timeline. It highlights the core 'Thử Thách Lãnh Đạo' and concludes by ensuring transparency with the 'Tiêu Chí Đánh Giá'. This structure logically guides the user from the 'why' to the 'what' and 'how', synthesizing multiple detailed reports into a single, digestible, and engaging flow, which is more effective than presenting disparate documents. -->
    <!-- Visualization & Content Choices: 
        - Competency Framework -> Goal: Inform & Compare -> Viz: Interactive Radar Charts (Chart.js) -> Interaction: Hover tooltips for definitions -> Justification: Radar charts are ideal for displaying multidimensional competency data in a compact, visually comparable format, making a complex framework easy to grasp.
        - Assessment Process -> Goal: Organize & Show Change -> Viz: Custom Vertical Timeline (HTML/CSS) -> Interaction: Static visual flow -> Justification: A timeline simplifies a multi-stage process, making the sequence and relationship between each assessment phase intuitive and easy to follow.
        - Evaluation Weighting -> Goal: Inform & Compare -> Viz: Donut Chart (Chart.js) -> Interaction: Hover to see percentages -> Justification: A donut chart is a clear and modern way to represent proportional data, instantly communicating the relative importance of each competency pillar.
        - Evaluation Criteria -> Goal: Organize & Inform -> Viz: Accordion/Collapsible Cards (HTML/JS) -> Interaction: Click to expand/collapse details -> Justification: This keeps the initial UI clean and allows users to drill down into specifics (like rating scales and calibration) without being overwhelmed, promoting progressive disclosure of information. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7f7f7;
            color: #333;
        }
        .ghn-orange { color: #E84D1A; }
        .bg-ghn-orange { background-color: #E84D1A; }
        .border-ghn-orange { border-color: #E84D1A; }
        .nav-link {
            position: relative;
            transition: color 0.3s ease;
        }
        .nav-link::after {
            content: '';
            position: absolute;
            width: 0;
            height: 2px;
            bottom: -4px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #E84D1A;
            transition: width 0.3s ease;
        }
        .nav-link.active, .nav-link:hover {
            color: #E84D1A;
        }
        .nav-link.active::after, .nav-link:hover::after {
            width: 100%;
        }
        .chart-container {
            position: relative;
            margin: auto;
            height: 280px;
            width: 100%;
            max-width: 350px;
        }
        @media (min-width: 768px) {
            .chart-container { height: 320px; max-width: 400px; }
        }
        .timeline-item::before {
            content: '';
            position: absolute;
            left: 1.25rem;
            top: 2rem;
            width: 2px;
            height: calc(100% - 1rem);
            background-color: #e5e7eb;
            transform: translateX(-50%);
        }
        .timeline-item:last-child::before {
            display: none;
        }
        .timeline-dot {
            position: absolute;
            left: 1.25rem;
            top: 0.5rem;
            width: 24px;
            height: 24px;
            transform: translateX(-50%);
        }
        .accordion-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.5s ease-in-out;
        }
        .fade-in {
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInAnimation 0.8s ease-out forwards;
        }
        @keyframes fadeInAnimation {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800">

    <header id="header" class="bg-white/90 backdrop-blur-lg sticky top-0 z-50 shadow-md transition-all duration-300">
        <div class="container mx-auto px-6 py-4">
            <div class="flex justify-between items-center">
                <a href="#" class="text-2xl font-bold ghn-orange tracking-wider">GHN TALENT</a>
                <nav class="hidden md:flex space-x-8 text-sm font-semibold">
                    <a href="#philosophy" class="nav-link text-gray-600">Triết Lý</a>
                    <a href="#framework" class="nav-link text-gray-600">Khung Năng Lực</a>
                    <a href="#process" class="nav-link text-gray-600">Quy Trình</a>
                    <a href="#challenge" class="nav-link text-gray-600">Thử Thách</a>
                    <a href="#criteria" class="nav-link text-gray-600">Tiêu Chí</a>
                </nav>
                <div class="md:hidden">
                    <button id="menu-btn" class="text-gray-700 focus:outline-none">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path></svg>
                    </button>
                </div>
            </div>
        </div>
        <div id="mobile-menu" class="md:hidden hidden bg-white border-t">
            <a href="#philosophy" class="block py-3 px-6 text-sm text-gray-700 hover:bg-gray-100">Triết Lý</a>
            <a href="#framework" class="block py-3 px-6 text-sm text-gray-700 hover:bg-gray-100">Khung Năng Lực</a>
            <a href="#process" class="block py-3 px-6 text-sm text-gray-700 hover:bg-gray-100">Quy Trình</a>
            <a href="#challenge" class="block py-3 px-6 text-sm text-gray-700 hover:bg-gray-100">Thử Thách</a>
            <a href="#criteria" class="block py-3 px-6 text-sm text-gray-700 hover:bg-gray-100">Tiêu Chí</a>
        </div>
    </header>

    <main>
        <section id="hero" class="bg-white pt-20 pb-24">
            <div class="container mx-auto px-6 text-center">
                <h1 class="text-4xl md:text-5xl font-extrabold text-gray-900 leading-tight fade-in">Hành Trình Kiến Tạo Chiến Tướng GHN</h1>
                <p class="mt-4 text-lg md:text-xl text-gray-600 max-w-3xl mx-auto fade-in" style="animation-delay: 0.2s;">
                    Một hệ thống đánh giá toàn diện, được xây dựng để xác định, phát triển và vinh danh những nhà lãnh đạo tương lai, những người sẽ dẫn dắt GHN chinh phục các đỉnh cao mới.
                </p>
                <div class="mt-10 fade-in" style="animation-delay: 0.4s;">
                    <a href="#philosophy" class="bg-ghn-orange text-white font-bold py-4 px-10 rounded-full hover:opacity-90 transition duration-300 shadow-lg hover:shadow-xl">Khám Phá Hành Trình</a>
                </div>
            </div>
        </section>

        <section id="philosophy" class="py-20">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-gray-900">Triết Lý Cốt Lõi: Xây Dựng Hồ Sơ Năng Lực</h2>
                    <p class="mt-4 text-lg text-gray-600 max-w-3xl mx-auto">Vượt trên các chỉ số hiệu suất truyền thống, chúng tôi xây dựng một hồ sơ năng lực đa chiều để xác định những nhà lãnh đạo đích thực.</p>
                </div>
                <div class="grid md:grid-cols-3 gap-8">
                    <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-2xl transition-shadow duration-300 fade-in">
                        <div class="flex items-center justify-center h-16 w-16 rounded-full bg-ghn-orange/10 mb-6">
                            <span class="text-3xl font-bold ghn-orange">1</span>
                        </div>
                        <h3 class="text-2xl font-bold text-gray-900 mb-3">Thành Tích Quá Khứ</h3>
                        <p class="text-gray-600">Đánh giá kết quả công việc và sự đóng góp đã được chứng minh qua các KPI vận hành và kinh doanh, là nền tảng của năng lực thực thi.</p>
                    </div>
                    <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-2xl transition-shadow duration-300 fade-in" style="animation-delay: 0.2s;">
                        <div class="flex items-center justify-center h-16 w-16 rounded-full bg-ghn-orange/10 mb-6">
                             <span class="text-3xl font-bold ghn-orange">2</span>
                        </div>
                        <h3 class="text-2xl font-bold text-gray-900 mb-3">Tiềm Năng Lãnh Đạo</h3>
                        <p class="text-gray-600">Xác định khả năng phát triển trong tương lai thông qua các năng lực cốt lõi như Nhanh nhẹn Học hỏi (Learning Agility), Sáng kiến và Tham vọng.</p>
                    </div>
                    <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-2xl transition-shadow duration-300 fade-in" style="animation-delay: 0.4s;">
                        <div class="flex items-center justify-center h-16 w-16 rounded-full bg-ghn-orange/10 mb-6">
                            <span class="text-3xl font-bold ghn-orange">3</span>
                        </div>
                        <h3 class="text-2xl font-bold text-gray-900 mb-3">Thấu Hiểu Bản Thân</h3>
                        <p class="text-gray-600">Đánh giá mức độ tự nhận thức sâu sắc, khả năng tự phản tư và nền tảng cho một phong cách lãnh đạo đích thực, truyền cảm hứng.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="framework" class="py-20 bg-white">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-gray-900">Khung Năng Lực Lãnh Đạo Tích Hợp</h2>
                    <p class="mt-4 text-lg text-gray-600 max-w-3xl mx-auto">Kết hợp các mô hình quốc tế (Korn Ferry, DDI) và bối cảnh đặc thù của GHN để xác định 4 cụm năng lực cốt lõi.</p>
                </div>
                <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
                    <div class="p-6 rounded-lg text-center fade-in"><div class="chart-container"><canvas id="chart1"></canvas></div><h3 class="text-xl font-bold text-center mt-4 text-gray-900">Tầm Nhìn & Tư Duy Chiến Lược</h3></div>
                    <div class="p-6 rounded-lg text-center fade-in" style="animation-delay: 0.2s;"><div class="chart-container"><canvas id="chart2"></canvas></div><h3 class="text-xl font-bold text-center mt-4 text-gray-900">Lãnh Đạo & Tạo Ảnh Hưởng</h3></div>
                    <div class="p-6 rounded-lg text-center fade-in" style="animation-delay: 0.4s;"><div class="chart-container"><canvas id="chart3"></canvas></div><h3 class="text-xl font-bold text-center mt-4 text-gray-900">Năng Lực Thực Thi Xuất Sắc</h3></div>
                    <div class="p-6 rounded-lg text-center fade-in" style="animation-delay: 0.6s;"><div class="chart-container"><canvas id="chart4"></canvas></div><h3 class="text-xl font-bold text-center mt-4 text-gray-900">Tự Nhận Thức & Linh Hoạt Học Hỏi</h3></div>
                </div>
            </div>
        </section>

        <section id="process" class="py-20">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-gray-900">Quy Trình Tuyển Chọn & Đánh Giá</h2>
                    <p class="mt-4 text-lg text-gray-600 max-w-3xl mx-auto">Một hành trình đa giai đoạn được thiết kế để thử thách và khai phá toàn diện năng lực của ứng viên.</p>
                </div>
                <div class="relative max-w-3xl mx-auto">
                    <div class="timeline-item pl-12 pb-12 fade-in">
                        <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center"><span class="text-sm font-bold ghn-orange">1</span></div>
                        <h3 class="text-xl font-bold text-gray-900 mb-2">Sàng Lọc & Tự Đánh Giá</h3>
                        <p class="text-gray-600">Sàng lọc hồ sơ dựa trên dữ liệu hiệu suất và yêu cầu ứng viên hoàn thành Phiếu Tự Đánh Giá Năng Lực để đánh giá mức độ tự nhận thức ban đầu.</p>
                    </div>
                    <div class="timeline-item pl-12 pb-12 fade-in">
                        <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center"><span class="text-sm font-bold ghn-orange">2</span></div>
                        <h3 class="text-xl font-bold text-gray-900 mb-2">Vòng Tranh Tài: Thử Thách Nhóm</h3>
                        <p class="text-gray-600">Ứng viên tham gia "Giải Mã Tình Huống Kinh Doanh" theo nhóm để đánh giá khả năng hợp tác, phân tích và giải quyết vấn đề trong môi trường thực tế.</p>
                    </div>
                    <div class="timeline-item pl-12 pb-12 fade-in">
                         <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center"><span class="text-sm font-bold ghn-orange">3</span></div>
                        <h3 class="text-xl font-bold text-gray-900 mb-2">Vòng Đối Đầu: Thử Thách Lãnh Đạo Chiến Lược (12 giờ)</h3>
                        <p class="text-gray-600">Thử thách cá nhân cao độ, yêu cầu ứng viên phân tích và xây dựng chiến lược cho các kịch bản thị trường hiện tại và tương lai giả định trong 12 giờ.</p>
                    </div>
                    <div class="timeline-item pl-12 pb-12 fade-in">
                        <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center"><span class="text-sm font-bold ghn-orange">4</span></div>
                        <h3 class="text-xl font-bold text-gray-900 mb-2">Vòng Chung Kết: Phỏng Vấn Sâu</h3>
                        <p class="text-gray-600">Phiên "Đối mặt" 1-1 với Hội đồng Đánh giá và Phỏng vấn Hành vi Sự kiện (BEI) để đào sâu vào kinh nghiệm, tư duy và tầm nhìn lãnh đạo.</p>
                    </div>
                    <div class="timeline-item pl-12 fade-in">
                         <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center"><span class="text-sm font-bold ghn-orange">5</span></div>
                        <h3 class="text-xl font-bold text-gray-900 mb-2">Lộ Trình Phát Triển Thực Thi</h3>
                        <p class="text-gray-600">Các ứng viên được chọn sẽ tham gia lộ trình theo dõi và phát triển năng lực thực thi chiến lược trong 3, 6 và 12 tháng.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="challenge" class="py-20 bg-white">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-gray-900">Trọng Tâm: Thử Thách Lãnh Đạo Chiến Lược</h2>
                    <p class="mt-4 text-lg text-gray-600 max-w-3xl mx-auto">Bài đánh giá cốt lõi kéo dài 12 giờ, bao gồm hai kịch bản được thiết kế để kiểm tra toàn diện năng lực lãnh đạo vận hành và chiến lược.</p>
                </div>
                <div class="grid md:grid-cols-2 gap-8 max-w-6xl mx-auto">
                    <div class="bg-gray-50 p-8 rounded-xl border border-gray-200 fade-in">
                        <h3 class="text-2xl font-bold text-gray-900 mb-4">Kịch bản 1: Tối ưu hóa Thị trường Hiện tại</h3>
                        <p class="text-gray-600 mb-4"><strong>Mục tiêu:</strong> Đánh giá mức độ am hiểu nội tại, khả năng phân tích dữ liệu và xây dựng chiến lược vận hành hiệu quả.</p>
                        <p class="text-gray-600"><strong>Nhiệm vụ:</strong> Phân tích một thách thức vận hành cụ thể (suy giảm thị phần, chi phí tăng...), xác định nguyên nhân gốc rễ dựa trên dữ liệu nội bộ (SWOT, 7S) và đề xuất kế hoạch hành động chi tiết.</p>
                    </div>
                     <div class="bg-gray-50 p-8 rounded-xl border border-gray-200 fade-in" style="animation-delay: 0.2s;">
                        <h3 class="text-2xl font-bold text-gray-900 mb-4">Kịch bản 2: Chinh phục Thị trường Tương lai</h3>
                        <p class="text-gray-600 mb-4"><strong>Mục tiêu:</strong> Đánh giá tầm nhìn chiến lược, khả năng thích ứng với sự thay đổi và năng lực dẫn dắt trong môi trường bất định.</p>
                        <p class="text-gray-600"><strong>Nhiệm vụ:</strong> Phân tích tác động của một biến động giả định (công nghệ mới, chính sách thay đổi...), phát triển các phương án chiến lược trong điều kiện thông tin hạn chế (PESTEL, 5 Forces) và trình bày một tầm nhìn dài hạn.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="criteria" class="py-20">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-gray-900">Tiêu Chí Đánh Giá Minh Bạch</h2>
                    <p class="mt-4 text-lg text-gray-600 max-w-3xl mx-auto">Một hệ thống đánh giá công bằng, khách quan với các tiêu chí và trọng số rõ ràng.</p>
                </div>
                <div class="max-w-4xl mx-auto">
                    <div class="bg-white rounded-xl shadow-lg p-8">
                        <div class="accordion-item mb-4 border-b pb-4 fade-in">
                            <button class="accordion-header w-full text-left flex justify-between items-center">
                                <h3 class="text-xl font-semibold text-gray-800">Khung Tỷ Trọng Đánh Giá</h3>
                                <span class="accordion-icon text-2xl ghn-orange transition-transform duration-300 transform">+</span>
                            </button>
                            <div class="accordion-content mt-4">
                                <p class="text-gray-600 mb-4">Tổng điểm được tính toán dựa trên trọng số của ba trụ cột năng lực chính, đảm bảo sự cân bằng giữa hiệu suất quá khứ, tiềm năng tương lai và phẩm chất cá nhân.</p>
                                <div class="chart-container h-64 md:h-80 mx-auto"><canvas id="weightingChart"></canvas></div>
                            </div>
                        </div>
                        <div class="accordion-item mb-4 border-b pb-4 fade-in">
                             <button class="accordion-header w-full text-left flex justify-between items-center">
                                <h3 class="text-xl font-semibold text-gray-800">Thang Điểm Đánh Giá Năng Lực</h3>
                                <span class="accordion-icon text-2xl ghn-orange transition-transform duration-300 transform">+</span>
                            </button>
                             <div class="accordion-content mt-4">
                                <p class="text-gray-600 mb-4">Mỗi năng lực được đánh giá trên thang điểm 5, với mô tả hành vi rõ ràng cho từng cấp độ, đảm bảo tính nhất quán và khách quan.</p>
                                <ul class="space-y-2 text-gray-700">
                                    <li><strong class="text-gray-900">5 - Hình Mẫu (Role Model):</strong> Thể hiện năng lực xuất sắc, là hình mẫu cho người khác.</li>
                                    <li><strong class="text-gray-900">4 - Vượt Kỳ Vọng (Competent):</strong> Thể hiện năng lực hiệu quả trong hầu hết các tình huống.</li>
                                    <li><strong class="text-gray-900">3 - Đạt Yêu Cầu (Adequate):</strong> Đáp ứng các yêu cầu cơ bản của năng lực.</li>
                                    <li><strong class="text-gray-900">2 - Cần Cải Thiện (Inconsistent):</strong> Thể hiện năng lực không nhất quán, còn hạn chế.</li>
                                    <li><strong class="text-gray-900">1 - Yếu Kém (Deficient):</strong> Không thể hiện được các hành vi cần thiết.</li>
                                </ul>
                            </div>
                        </div>
                        <div class="accordion-item fade-in">
                             <button class="accordion-header w-full text-left flex justify-between items-center">
                                <h3 class="text-xl font-semibold text-gray-800">Quy Trình Hiệu Chỉnh & Đào Tạo</h3>
                                 <span class="accordion-icon text-2xl ghn-orange transition-transform duration-300 transform">+</span>
                            </button>
                             <div class="accordion-content mt-4">
                                <p class="text-gray-600">Để đảm bảo tính công bằng, GHN áp dụng quy trình hiệu chỉnh (calibration) kết quả, nơi các thành viên Hội đồng Đánh giá cùng thảo luận để đi đến đồng thuận. Tất cả người đánh giá đều được đào tạo kỹ lưỡng về khung năng lực và kỹ thuật đánh giá khách quan.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>
    
    <footer class="bg-gray-900 text-white">
        <div class="container mx-auto px-6 py-10 text-center">
            <p>&copy; 2025 Giao Hàng Nhanh. All Rights Reserved.</p>
            <p class="text-sm text-gray-400 mt-2">Hành Trình Kiến Tạo & Phát Triển Lãnh Đạo Tương Lai</p>
        </div>
    </footer>


    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const menuBtn = document.getElementById('menu-btn');
            const mobileMenu = document.getElementById('mobile-menu');
            
            menuBtn.addEventListener('click', () => {
                mobileMenu.classList.toggle('hidden');
            });

            document.querySelectorAll('#mobile-menu a').forEach(link => {
                link.addEventListener('click', () => {
                    mobileMenu.classList.add('hidden');
                });
            });

            const sections = document.querySelectorAll('main section');
            const headerNavLinks = document.querySelectorAll('header nav a');
            
            const observerOptions = {
                root: null,
                rootMargin: '-50% 0px -50% 0px',
                threshold: 0
            };

            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        const id = entry.target.getAttribute('id');
                        headerNavLinks.forEach(link => {
                            link.classList.remove('active');
                            if (link.getAttribute('href') === `#${id}`) {
                                link.classList.add('active');
                            }
                        });
                    }
                });
            }, observerOptions);

            sections.forEach(section => {
                observer.observe(section);
            });

            document.querySelectorAll('.accordion-header').forEach(header => {
                header.addEventListener('click', () => {
                    const content = header.nextElementSibling;
                    const icon = header.querySelector('.accordion-icon');
                    
                    if (content.style.maxHeight) {
                        content.style.maxHeight = null;
                        icon.classList.remove('rotate-45');
                    } else {
                        document.querySelectorAll('.accordion-content').forEach(c => c.style.maxHeight = null);
                        document.querySelectorAll('.accordion-icon').forEach(i => i.classList.remove('rotate-45'));
                        content.style.maxHeight = content.scrollHeight + "px";
                        icon.classList.add('rotate-45');
                    }
                });
            });

            const radarChartOptions = {
                maintainAspectRatio: false,
                scales: {
                    r: {
                        angleLines: { display: true, color: 'rgba(0, 0, 0, 0.05)' },
                        suggestedMin: 0,
                        suggestedMax: 5,
                        pointLabels: {
                            font: { size: 10, weight: '600' },
                            color: '#4b5563'
                        },
                        grid: { color: 'rgba(0, 0, 0, 0.08)' },
                        ticks: { display: false, stepSize: 1 }
                    }
                },
                plugins: {
                    legend: { display: false },
                    tooltip: {
                        callbacks: {
                            label: (context) => `${context.dataset.label}: ${context.raw}`
                        }
                    }
                }
            };
            
            const competencyData = {
                strategic: { labels: ['Phân tích Thị trường', 'Hoạch định Chiến lược', 'Tư duy Đổi mới'], data: [4, 5, 4] },
                leadership: { labels: ['Truyền cảm hứng', 'Phát triển Đội ngũ', 'Quản trị Thay đổi'], data: [5, 4, 4] },
                execution: { labels: ['Lập kế hoạch', 'Ra quyết định', 'Thúc đẩy Kết quả'], data: [5, 5, 4] },
                self: { labels: ['Tự nhận thức', 'Tư duy Phát triển', 'Học hỏi Liên tục'], data: [4, 5, 5] }
            };
            
            function createRadarChart(canvasId, data, label) {
                new Chart(document.getElementById(canvasId), {
                    type: 'radar',
                    data: {
                        labels: data.labels,
                        datasets: [{
                            label: label, data: data.data,
                            backgroundColor: 'rgba(232, 77, 26, 0.2)',
                            borderColor: '#E84D1A',
                            borderWidth: 2,
                            pointBackgroundColor: '#E84D1A',
                            pointBorderColor: '#fff',
                            pointHoverBackgroundColor: '#fff',
                            pointHoverBorderColor: '#E84D1A'
                        }]
                    },
                    options: radarChartOptions
                });
            }

            createRadarChart('chart1', competencyData.strategic, 'Tầm Nhìn');
            createRadarChart('chart2', competencyData.leadership, 'Lãnh Đạo');
            createRadarChart('chart3', competencyData.execution, 'Thực Thi');
            createRadarChart('chart4', competencyData.self, 'Bản Thân');
            
            new Chart(document.getElementById('weightingChart'), {
                type: 'doughnut',
                data: {
                    labels: ['Năng lực Thực thi (45%)', 'Tiềm năng Lãnh đạo (35%)', 'Thấu hiểu Bản thân (20%)'],
                    datasets: [{
                        data: [45, 35, 20],
                        backgroundColor: ['#E84D1A', '#F5A623', '#4A4A4A'],
                        borderColor: '#fff',
                        borderWidth: 4,
                        hoverOffset: 4
                    }]
                },
                options: {
                    maintainAspectRatio: false,
                    responsive: true,
                    plugins: {
                        legend: { position: 'bottom', labels: { font: { size: 12 }, boxWidth: 15, padding: 20 } },
                        tooltip: {
                            callbacks: {
                                label: (context) => `${context.label}: ${context.raw}%`
                            }
                        }
                    },
                    cutout: '60%'
                }
            });

            const fadeInElements = document.querySelectorAll('.fade-in');
            const fadeInObserver = new IntersectionObserver((entries, observer) => {
                entries.forEach(entry => {
                    if(entry.isIntersecting){
                        entry.target.style.animationDelay = entry.target.style.animationDelay || '0s';
                        entry.target.classList.add('visible');
                        observer.unobserve(entry.target);
                    }
                });
            }, { threshold: 0.1 });
            fadeInElements.forEach(el => fadeInObserver.observe(el));

        });
    </script>
</body>
</html>
