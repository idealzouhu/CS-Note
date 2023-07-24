# 结构体初始化

结构体

```
typedef struct {
    const SKP_int32             nStages;

    /* Fields for (de)quantizing */
    const SKP_Silk_NLSF_CBS     *CBStages;
    const SKP_int               *NDeltaMin_Q15;

    /* Fields for arithmetic (de)coding */
    const SKP_uint16            *CDF;
    const SKP_uint16 * const    *StartPtr;
    const SKP_int               *MiddleIx;
} SKP_Silk_NLSF_CB_struct;
```

初始化

```
const SKP_Silk_NLSF_CB_struct SKP_Silk_NLSF_CB0_10 =
{
        NLSF_MSVQ_CB0_10_STAGES,
        SKP_Silk_NLSF_CB0_10_Stage_info,
        SKP_Silk_NLSF_MSVQ_CB0_10_ndelta_min_Q15,
        SKP_Silk_NLSF_MSVQ_CB0_10_CDF,
        SKP_Silk_NLSF_MSVQ_CB0_10_CDF_start_ptr,
        SKP_Silk_NLSF_MSVQ_CB0_10_CDF_middle_idx
};
```

