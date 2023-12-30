import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class Album {
  final int id;
  final String title;

  Album({required this.id, required this.title});
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Album App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: AlbumScreen(),
    );
  }
}

class AlbumScreen extends StatefulWidget {
  @override
  _AlbumScreenState createState() => _AlbumScreenState();
}

class _AlbumScreenState extends State<AlbumScreen> {
  List<Album> albums = [
    Album(id: 1, title: 'Album 1'),
    Album(id: 2, title: 'Album 2'),
    Album(id: 3, title: 'Album 3'),
  ];
  TextEditingController _controller = TextEditingController();

  void addAlbum(String title) {
    final int newId = albums.length + 1;
    setState(() {
      albums.add(Album(id: newId, title: title));
    });
  }

  void deleteAlbum(int id) {
    setState(() {
      albums.removeWhere((album) => album.id == id);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Albums'),
      ),
      body: ListView.builder(
        itemCount: albums.length,
        itemBuilder: (BuildContext context, int index) {
          return ListTile(
            title: Text(albums[index].title),
            trailing: IconButton(
              icon: Icon(Icons.delete),
              onPressed: () {
                deleteAlbum(albums[index].id);
              },
            ),
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          showDialog(
            context: context,
            builder: (BuildContext context) {
              return AlertDialog(
                title: Text('Add Album'),
                content: TextField(
                  controller: _controller,
                  decoration: InputDecoration(hintText: 'Enter album title'),
                ),
                actions: [
                  TextButton(
                    onPressed: () {
                      Navigator.of(context).pop();
                      addAlbum(_controller.text);
                      _controller.clear();
                    },
                    child: Text('Add'),
                  ),
                ],
              );
            },
          );
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
