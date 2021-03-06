# flutter_chips_input

Flutter library for building input fields with InputChips as input options.

## Usage

### Installation
Follow installation instructions [here](https://pub.dartlang.org/packages/flutter_chips_input#-installing-tab-)

### Example
```
ChipsInput(
    initialValue: [
        AppProfile('John Doe', 'jdoe@flutter.io', 'https://d2gg9evh47fn9z.cloudfront.net/800px_COLOURBOX4057996.jpg')
    ],
    decoration: InputDecoration(
        labelText: "Select People",
    ),
    findSuggestions: (String query) {
        if (query.length != 0) {
            var lowercaseQuery = query.toLowerCase();
            return mockResults.where((profile) {
                return profile.name.toLowerCase().contains(query.toLowerCase()) || profile.email.toLowerCase().contains(query.toLowerCase());
            }).toList(growable: false)
                ..sort((a, b) => a.name
                    .toLowerCase()
                    .indexOf(lowercaseQuery)
                    .compareTo(b.name.toLowerCase().indexOf(lowercaseQuery)));
        } else {
            return const <AppProfile>[];
        }
    },
    onChanged: (data) {
        print(data);
    },
    chipBuilder: (context, state, profile) {
        return InputChip(
            key: ObjectKey(profile),
            label: Text(profile.name),
            avatar: CircleAvatar(
                backgroundImage: NetworkImage(profile.imageUrl),
            ),
            onDeleted: () => state.deleteChip(profile),
                materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
        );
    },
    suggestionBuilder: (context, state, profile) {
        return ListTile(
            key: ObjectKey(profile),
            leading: CircleAvatar(
                backgroundImage: NetworkImage(profile.imageUrl),
            ),
            title: Text(profile.name),
            subtitle: Text(profile.email),
            onTap: () => state.selectSuggestion(profile),
        );
    },
)
```

## Known Issues
* Overlay doesn't move when input height changes i.e. when chips wrap
* For some reason Overlay floats above AppBar when scrolling
