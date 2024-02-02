在输出loss的时候会自动按照window size给你做一个平均，而不是显示当的loss
```python
log_processor = dict(by_epoch=True, type='LogProcessor', window_size=75,
                     custom_cfg=[dict(
                        data_src='loss_mask',
                        log_name='loss_mask_current',
                        method_name='current')])
```
最好按照上面这么设置，才会返回一个当前的loss值
