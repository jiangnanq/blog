---
title: UITableView controller
date: 2019-10-03
layout: post
tags: Swift, iOS
---

## Note of UITableview controller

* Basic usage

```swift
extension ViewController: UITableViewDelegate, UITableViewDataSource {
  func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
      return messages.count
  }
  
  func numberOfSections(in tableView: UITableView) -> Int {
      return 1
  }
  
  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
      let cell=tableView.dequeueReusableCell(withIdentifier: "message", for: indexPath)
      cell.textLabel?.text = messages[indexPath.row]
      return cell
  }
}
```

* Select cell and send data to next viewcontroller

```swift
let vc = self.storyboard?.instantiateViewControllerWithIdentifier("studentDetailViewController") as? studentDetailViewController
vc?.newStudent = false
vc?.currentStudent = DataManager.copyStudent(Students[indexPath.row])
self.navigationController?.pushViewController(vc!, animated: true)  
```

* Select cell and prepare to segue

```swift
  override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
      if segue.identifier == "showdetail" {
          let vc = segue.destination as! detailViewController
          vc.index = (bedCollectionView.indexPathsForSelectedItems?[0].row)!
      }
  } 
```

* Use custom cell in tableview controller

```swift
let cell = collectionView.dequeueReusableCellWithReuseIdentifier(reuseIdentifier, forIndexPath: indexPath) as! checkListCollectionViewCell
```

* Delete row from tableview controller. Update UIView in other queue. Update tableview by use tableview.deleteRowAtIndexpath

```swift
extension savedSongViewController:UITableViewDelegate {
  func tableView(savedSongTable: UITableView, canEditRowAtIndexPath indexPath: NSIndexPath) -> Bool {
      return true
  }
  
  func tableView(savedSongTable: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
      if editingStyle == .Delete {
          self.allSavedSongs.removeAtIndex(indexPath.row)
          self.savedSongs?.favoriteTracks.removeAtIndex(indexPath.row)
          self.savedSongs?.saveAllSongs()
          var selectIndex = [NSIndexPath]()
          selectIndex.append(indexPath)
          savedSongTable.deleteRowsAtIndexPaths(selectIndex, withRowAnimation: UITableViewRowAnimation.Fade)
          dispatch_async(dispatch_get_main_queue(), { () -> Void in
              self.savedSongTable?.reloadData()
          })            
      }
  }
}
```

* Detect button/image was tapped in a cell (use protocol)

[Link](http://candycode.io/how-to-properly-do-buttons-in-table-view-cells/)

* Click a cell and popup alertcontroller

```swift
  func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
      let i = indexPath.row
      print ("\(i)")
      let alertController = UIAlertController(title: "Bed", message: "Select", preferredStyle: .actionSheet)
      
      let selectAction = UIAlertAction(title: "Detail", style: .default) { (_) in
          print ("show detail")
      }
      let cancelAction = UIAlertAction(title: "Cancel", style: .default, handler: nil)
      alertController.addAction(selectAction)
      alertController.addAction(cancelAction)
      let popOver = alertController.popoverPresentationController
      popOver?.sourceView = collectionView.cellForItem(at: indexPath)
      popOver?.sourceRect = collectionView.cellForItem(at: indexPath)!.bounds
      popOver?.permittedArrowDirections = UIPopoverArrowDirection.any
      
      present(alertController, animated: true, completion: nil)
  }
```

* UITableView cell auto height

1. Add constrains in the cell's view
2. Add below code in the viewcontroller 

```swift
  self.tableView.rowHeight = UITableViewAutomaticDimension
  override func tableView(_ tableView: UITableView, estimatedHeightForRowAt indexPath: IndexPath) -> CGFloat {
  return 140
}
```

* Reload one cell in the tableview

```swift
  let ip = NSIndexPath(row: row, section: 0)
  self.tableView.reloadRows(at: [ip as IndexPath], with: .top)
```

* Refresh control usage

```swift
    self.refreshControl = UIRefreshControl()
    self.refreshControl.addTarget(self, action: #selector(test), for: .valueChanged)
```
