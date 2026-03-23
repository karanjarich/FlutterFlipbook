import 'package:flutter/material.dart';
import 'package:turnable_page/turnable_page.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Soma Safari - Travel Book',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: _AnimationTestPage(),
    );
  }
}

class _AnimationTestPage extends StatefulWidget {
  const _AnimationTestPage();

  @override
  _AnimationTestPageState createState() => _AnimationTestPageState();
}

class _AnimationTestPageState extends State<_AnimationTestPage> {
  late PageFlipController _controller;

  // Updated with Pexels travel image URLs
  final List<String> _displayItemsFromMockApi = [
    "https://images.pexels.com/photos/2166559/pexels-photo-2166559.jpeg",
    "https://images.pexels.com/photos/3278215/pexels-photo-3278215.jpeg",
    "https://images.pexels.com/photos/2387873/pexels-photo-2387873.jpeg",
    "https://images.pexels.com/photos/258154/pexels-photo-258154.jpeg",
    "https://images.pexels.com/photos/1007657/pexels-photo-1007657.jpeg",
    "https://images.pexels.com/photos/457882/pexels-photo-457882.jpeg",
    "https://images.pexels.com/photos/1450353/pexels-photo-1450353.jpeg",
    "https://images.pexels.com/photos/210186/pexels-photo-210186.jpeg",
  ];

  final ValueNotifier<int> _currentPageNotifier = ValueNotifier<int>(0);
  final ValueNotifier<int> _flipCountNotifier = ValueNotifier<int>(0);

  @override
  void initState() {
    super.initState();
    _controller = PageFlipController();
  }

  @override
  void dispose() {
    _currentPageNotifier.dispose();
    _flipCountNotifier.dispose();
    super.dispose();
  }

  Widget _buildLoadingPage() {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        mainAxisSize: MainAxisSize.min,
        children: [
          CircularProgressIndicator(color: Colors.blueAccent),
          SizedBox(height: 20),
          Text(
            "Loading Safari Adventures...",
            style: TextStyle(color: Colors.blueAccent),
          ),
        ],
      ),
    );
  }

  Widget _buildTestPage(int index, BoxConstraints constraints) {
    return Container(
      color: Colors.black, // Dark background for the photos
      child: Stack(
        fit: StackFit.expand,
        children: [
          // The Travel Image
          Image.network(
            _displayItemsFromMockApi[index],
            fit: BoxFit.cover,
            loadingBuilder: (context, child, loadingProgress) {
              if (loadingProgress == null) return child;
              return Center(child: CircularProgressIndicator());
            },
          ),

          // Gradient Overlay for Text Visibility
          Container(
            decoration: BoxDecoration(
              gradient: LinearGradient(
                begin: Alignment.bottomCenter,
                end: Alignment.topCenter,
                colors: [Colors.black.withOpacity(0.8), Colors.transparent],
              ),
            ),
          ),

          // Content Layout
          Padding(
            padding: const EdgeInsets.all(24.0),
            child: Column(
              mainAxisAlignment: MainAxisAlignment.end,
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Destination ${index + 1}',
                  style: TextStyle(
                    fontSize: 36,
                    fontWeight: FontWeight.bold,
                    color: Colors.white,
                  ),
                ),
                SizedBox(height: 8),
                Text(
                  'Explore the beauty of the world through our flipbook.',
                  style: TextStyle(fontSize: 18, color: Colors.white70),
                ),
                SizedBox(height: 20),
                ElevatedButton.icon(
                  onPressed: () {
                    ScaffoldMessenger.of(context).showSnackBar(
                      SnackBar(content: Text('Viewing destination details...')),
                    );
                  },
                  icon: Icon(Icons.map),
                  label: Text('Explore Now'),
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.white.withOpacity(0.9),
                    foregroundColor: Colors.black,
                  ),
                ),
              ],
            ),
          ),

          // Page info overlay
          Positioned(
            top: 40,
            right: 20,
            child: ValueListenableBuilder<int>(
              valueListenable: _currentPageNotifier,
              builder: (context, currentPage, child) {
                return Container(
                  padding: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
                  decoration: BoxDecoration(
                    color: Colors.black54,
                    borderRadius: BorderRadius.circular(20),
                  ),
                  child: Text(
                    '${currentPage + 1} / ${_displayItemsFromMockApi.length}',
                    style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold),
                  ),
                );
              },
            ),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.grey[900],
      body: Center(
        child: TurnablePage(
          key: UniqueKey(),
          controller: _controller,
          pageCount: _displayItemsFromMockApi.length,
          pageViewMode: PageViewMode.single,
          paperBoundaryDecoration: PaperBoundaryDecoration.modern,
          settings: FlipSettings(
            hideLeftShadow: false,
            onlyVerticalPageFlip: false,
            drawShadow: true,
            flippingTime: 1000,
            swipeDistance: 50.0,
            cornerTriggerAreaSize: 0.2,
            usePortrait: true,
          ),
          onPageChanged: (leftPageIndex, rightPageIndex) {
            _currentPageNotifier.value = rightPageIndex;
            _flipCountNotifier.value = _flipCountNotifier.value + 1;
          },
          builder: (context, pageIndex, constraints) {
            return _buildTestPage(pageIndex, constraints);
          },
        ),
      ),
    );
  }
}
