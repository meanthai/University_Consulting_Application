import 'package:flutter/material.dart';
void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'University Consulting Application',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: UniversityFinder(),
    );
  }
}
// launching cục bộ app

class University {
  final String name;
  final String major;
  final double entrancePoints;
  final double fee;

  University({
    required this.name,
    required this.major,
    required this.entrancePoints,
    required this.fee,
  });
}
/// khai báo object University lưu các characteristics như tên trường, ngành, điểm đầu vào, học phí

class UniversityFinder extends StatefulWidget {
  @override
  _UniversityFinderState createState() => _UniversityFinderState();
}


/// class bên dưới sẽ xử lý chính cũng như tạo các functions
class _UniversityFinderState extends State<UniversityFinder> with TickerProviderStateMixin {
  final TextEditingController pointController = TextEditingController();
  final TextEditingController budgetController = TextEditingController();
  // này là sẽ lấy 2 dữ liệu là điểm và ngân sách từ hai ô nhập input

  String selectedMajor = 'Công nghệ thông tin'; // mặc định lấy từ hàm DropdownButtonFormField<String> function

  List<String> selectedUniversities = []; // dach sách các trường sẽ chọn
  bool isResultsVisible = false; 

  List<University> universities = [
    University(name: 'UIT - Trường Đại học Công nghệ Thông tin', major: "Công nghệ thông tin", entrancePoints: 27.2, fee: 40),
    University(name: 'HCMUS - Trường Đại học Khoa học Tự nhiên', major: "Công nghệ thông tin", entrancePoints: 26.8, fee: 45),
    University(name: 'HCMUT - Trường Đại học Bách khoa', major: "Công nghệ thông tin", entrancePoints: 25, fee: 43),
    //Tech
    University(name: 'FTU - Trường Đại học Ngoại Thương', major: "Kinh tế", entrancePoints: 28, fee: 48),
    University(name: 'UEH - Trường Đại học Kinh tế Thành phố Hồ Chí Minh', major: "Kinh tế", entrancePoints: 26, fee: 46),
    University(name: 'UEF - Trường Đại học Kinh Tế Tài chính', major: "Kinh tế", entrancePoints: 24, fee: 40),
    //Economics
    University(name: 'QNU - Trường Đại học Quy Nhơn', major: "Du lịch", entrancePoints: 22, fee: 30),
    University(name: 'DNU - Trường Đại học Đà Nẵng', major: "Du lịch", entrancePoints: 21, fee: 26),
    //Tourism
    University(name: 'HUB - Trường Đại học Ngân hàng Thành phố Hồ Chí Minh', major: "Ngân hàng", entrancePoints: 24, fee: 31),
    University(name: 'AOB - Học viện Ngân hàng', major: "Ngân hàng", entrancePoints: 26, fee: 36),
    //Banking
  ];
  /// này là danh sách data để lưu tên trường rồi các characteristics

  late AnimationController _animationController;
  late Animation<double> _opacityAnimation;

  @override
  void initState() {
    super.initState();
    _animationController = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 1000),
      reverseDuration: const Duration(milliseconds: 1000),
    );
    _opacityAnimation = Tween<double>(begin: 0.0, end: 1.0).animate(_animationController);
  }/// này để chỉnh visual effects (ở đây là fade-out effect)
  

  void findUniversities() {
    double entrancePoints = double.tryParse(pointController.text.trim()) ?? 0;
    double budget = double.tryParse(budgetController.text.trim()) ?? 0;
    // chuyển đổi dữ liệu input data ở dạng text thành double cho tương thích (hàm chuyển tryparse)

    List<String> selected = [];

    for (var university in universities) {
      if (entrancePoints >= university.entrancePoints && budget >= university.fee && selectedMajor == university.major) {
        selected.add(university.name);
      }
    }

    setState(() {
      selectedUniversities = selected.isEmpty ? ['Rất tiếc, bạn nghèo và dốt vl'] : selected; // kiểm tra xem có chọn được trường hay không
      isResultsVisible = true; // cái này nó sẽ xuất hiện ra danh sách kết quả sau khi xử lý
      _animationController.forward(from: 0);// bắt đầu set time cho animation
    });
  }///hàm tìm các trường thích hợp dựa trên ngành đã chọn và số điểm, kinh phí từ input data


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        flexibleSpace: Container(
          color: Color.fromARGB(255, 168, 54, 244), 
          child: const FlexibleSpaceBar(
            title: Text('University Consulting Application'),
          ),
        ),
      ),
      /// tạo thanh toolbar chứa tên app

      body: Padding(
        padding: const EdgeInsets.all(20.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [

            const Text(
              'Author: Hoàng Minh Thái',
              style: TextStyle(
                    fontSize: 14, // thay đổi size của font chữ nè
                    color: Color.fromARGB(255, 168, 54, 244), // thay đổi màu nè
                  ),
              ),
              const SizedBox(height: 30),
              /// tạo dòng author: Hoàng Minh Thái mặc định layout bên trái

            const Center(
              child: Column(
                children: [
                  Text(
                    'Chào mừng bạn đến với ứng dụng tư vấn tuyển sinh trường đại học',
                    style: TextStyle(
                      fontSize: 30,
                      fontWeight: FontWeight.bold,
                      color: Color.fromARGB(255, 244, 54, 54),
                    ),
                  ),
                  SizedBox(height: 10),
                ],
              ),
            ),
            /// tạo dòng chào mừng đến app chỉnh layout center
            
            const SizedBox(
                width: 66,
                height: 90,
                child: Icon(
                  Icons.wb_sunny,
                  size: 45,
                  color: Colors.orange,
                ),
              ),
              /// tạo icon mặt trời cute cute
            

            DropdownButtonFormField<String>(
              value: selectedMajor,
              items: ['Công nghệ thông tin', 'Kinh tế', 'Du lịch', 'Ngân hàng'].map((String major) {
                return DropdownMenuItem<String>(
                  value: major,
                  child: Text(
                    major,
                    style: const TextStyle(
                      fontSize: 14, 
                    ),
                  ),
                );
              }).toList(),
              // tạo danh sách items là các ngành và giá trị selectedmajor

              onChanged: (String? value) {
                setState(() {
                  selectedMajor = value ?? 'Công nghệ thông tin';
                  // mặc định nhé nếu biến value có giá trị null thì ngành chọn sẽ tự động chọn cntt
                });
              },

              decoration: const InputDecoration(
                labelText: 'Hãy chọn ngành yêu thích của bạn:', 
                labelStyle: TextStyle(
                  color: Color.fromARGB(255, 243, 145, 33),
                  fontSize: 24,       
                ),
              ),
            ),
            const SizedBox(height: 25),
            /// tạo danh sách các ngành sẽ chọn và lấy dữ liệu đó làm input data cho hàm xử lý

            TextField(
              controller: pointController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: 'Hãy nhập số điểm bạn ước tính (bằng số từ 0.0 tới 30.0):', 
                labelStyle: TextStyle(
                  color: Color.fromARGB(255, 243, 145, 33),
                  fontSize: 19,      
                ),
              ),
            ),
            const SizedBox(height: 25),
            // tạo ô nhập điểm để lấy input data

            TextField(
              controller: budgetController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: 'Hãy nhập vào số tiền bạn có thể chi trả cho một năm học (Triệu đồng):', 
                labelStyle: TextStyle(
                  color: Color.fromARGB(255, 243, 145, 33),
                  fontSize: 19,       
                ),
              ),
            ),
            const SizedBox(height: 25),
            // tạo ô nhập kinh phí để lấy input data

            ElevatedButton(
              onPressed: findUniversities,
              child: const Text(
                'Tìm trường đại học thích hợp',
                style: TextStyle(
                    fontSize: 18, 
                    color: Color.fromARGB(255, 168, 54, 244), 
                  ),
                ),
            ),
            const SizedBox(height: 25),
            // nút tìm trường kích hoạt function findUniversity()

            AnimatedBuilder(
              animation: _opacityAnimation,
              builder: (context, child) {
                return Visibility(
                  visible: isResultsVisible,
                  child: Column(
                    children: [

                      const Text(
                        'Danh sách các trường đáp ứng tiêu chí của bạn:',
                        style: TextStyle(fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      // in ra text danh sách các trường

                      for (var university in selectedUniversities)
                        FadeTransition(
                          opacity: _opacityAnimation,
                          child: Text(
                            university,
                            style: const TextStyle(
                              fontSize: 20, 
                              color: Colors.red, 
                            ),
                          ),
                        ),
                        // in ra các trường thỏa mãn tiêu chí cũng như kích hoạt animation fade-out
                    ],
                  ),
                );
                //hàm kích hoạt hiển thị kết quả
              },
            ),
          ],
        ),
      ),
    );
  }

  @override
  void dispose() {
    _animationController.dispose();
    super.dispose();
  }
}
