
训练命令:`python3 test.py --cfg_file cfgs/kitti_models/VFF_PVRCNN.yaml --batch_size 1 --ckpt ../VFF_PVRCNN.pth`  

-------------------------------------  
修改部分：
1. 报错如下  
```
Traceback (most recent call last):
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/tools/test.py", line 199, in <module>
    main()
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/tools/test.py", line 195, in main
    eval_single_ckpt(model, test_loader, args, eval_output_dir, logger, epoch_id, dist_test=dist_test)
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/tools/test.py", line 61, in eval_single_ckpt
    eval_utils.eval_one_epoch(
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/tools/eval_utils/eval_utils.py", line 59, in eval_one_epoch
    pred_dicts, ret_dict = model(batch_dict)
  File "/home/xilm/anaconda3/lib/python3.9/site-packages/torch/nn/modules/module.py", line 1102, in _call_impl
    return forward_call(*input, **kwargs)
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/pcdet/models/detectors/pv_rcnn_fusion.py", line 11, in forward
    batch_dict = cur_module(batch_dict)
  File "/home/xilm/anaconda3/lib/python3.9/site-packages/torch/nn/modules/module.py", line 1102, in _call_impl
    return forward_call(*input, **kwargs)
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/pcdet/models/backbones_3d/vfe/image_point_vfe.py", line 104, in forward
    batch_dict = self.backbone_3d(batch_dict, fuse_func=self.f2v)
  File "/home/xilm/anaconda3/lib/python3.9/site-packages/torch/nn/modules/module.py", line 1102, in _call_impl
    return forward_call(*input, **kwargs)
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/pcdet/models/backbones_3d/spconv_backbone.py", line 502, in forward
    x_conv1, batch_dict = self.fusion(fuse_func, batch_dict, x_conv1, "layer1")
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/pcdet/models/backbones_3d/spconv_backbone.py", line 469, in fusion
    voxel_feat, batch_dict = fuse_func(batch_dict, encoded_voxel=voxel_feat, 
  File "/home/xilm/anaconda3/lib/python3.9/site-packages/torch/nn/modules/module.py", line 1102, in _call_impl
    return forward_call(*input, **kwargs)
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/pcdet/models/backbones_3d/vfe/image_vfe_modules/f2v/voxel_field_fusion.py", line 162, in forward
    projection_dict = self.point_projector(voxel_coords=encoded_voxel.indices.float(),
  File "/home/xilm/anaconda3/lib/python3.9/site-packages/torch/nn/modules/module.py", line 1102, in _call_impl
    return forward_call(*input, **kwargs)
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/pcdet/models/backbones_3d/vfe/image_vfe_modules/f2v/point_to_image_projection.py", line 161, in forward
    image_grid, image_depths, batch_voxel, batch_mask = self.transform_grid(voxel_coords=voxel_coords, 
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/pcdet/models/backbones_3d/vfe/image_vfe_modules/f2v/point_to_image_projection.py", line 115, in transform_grid
    image_grid, image_depths = transform_utils.project_to_image(project=I_C, points=camera_grid, bmm=True)
  File "/home/xilm/fuxian/Voxel_Field_Fusion/VFF-main/pcdet/utils/transform_utils.py", line 44, in project_to_image
    points_depth = points_t[..., -1] - raw_project[..., 2, 3]
RuntimeError: The size of tensor a (15463) must match the size of tensor b (2) at non-singleton dimension 1
```  
修改方法：  
将`Voxel_Field_Fusion/VFF-main/pcdet/utils/transform_utils.py`文件中44行代码`points_depth = points_t[..., -1] - raw_project[..., 2, 3]`修改为  
```
raw_project_add = raw_project[..., 2, 3].view(points_t[..., -1].shape[0], -1)
points_depth = points_t[..., -1] - raw_project_add
```  
