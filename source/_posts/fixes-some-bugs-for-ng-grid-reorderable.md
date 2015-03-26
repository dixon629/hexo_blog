title: "Fixes some bugs for ng-grid-reorderable"
date: 2015-03-26 22:00:47
categories: angularjs
tags: [ng-gird,angularjs]
---
[ng-grid-reorderable](https://github.com/angular-ui/ng-grid/tree/2.x/plugins) is a drag-drop plugin for [ng-gird](https://github.com/angular-ui/ng-grid). I found some bugs when I was using this plugin.
###Bug 1: Can drag a row to the other ng-grid
To fix this bug, it needs to compare grid id on drop event.
```javascript  
    var prevRow = self.services.DomUtilityService.eventStorage.rowToMove;
    if (prevRow.gridId != self.myGrid.gridId) return;
```
###Bug 2: Can't select all texts when at edting status
To fix this bug, it needs to disable drag-drop feature.
```javascript
            //It should disable drag/drop feature when editing
            rowScope.$on('ngGridEventStartCellEdit', function () {
                targetRow.attr('draggable', 'false');
                targetRow.off('dragstart', onDragStart);
            });
            rowScope.$on('ngGridEventEndCellEdit', function () {
                targetRow.attr('draggable', 'true');
                targetRow.on('dragstart', onDragStart);
            });
```
###Bug 3: Can select some texts and then drag and drop it on the ng-gird
To fix this bug, it needs to remove dragover and drop event for the $viewport when editing
```javascript
            rowScope.$on('ngGridEventStartCellEdit', function () {
                self.myGrid.$viewport.off('dragover', self.dragOver).off('drop', self.onRowDrop);
            });
            rowScope.$on('ngGridEventEndCellEdit', function () {
                self.myGrid.$viewport.on('dragover', self.dragOver).on('drop', self.onRowDrop);
            });
```
###The codes in gist
{% gist a77d1507f0e65c0e2908 ng-grid-reorderable.js %}
