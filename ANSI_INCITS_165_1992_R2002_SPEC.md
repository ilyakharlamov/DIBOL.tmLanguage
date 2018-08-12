
# DIBOL Syntactic Specification

## 19.1 Conventions

The following conventions shall be used in defining the DIBOL syntax:
 1. Each syntax specification shall have the general form of
< left_side > := < right_side >
which should be read as " < left side > shall be defined as < right_side >," where < left side > shall be a logical syntactical element and < right_side > shall be one or more definition constructs that define the syntax for that element. If there is more than one construct, the constructs are separated by the " |" character and denote a list from which one construct shall be chosen.
 2. Each definition construct consists of one or more syntactic elements. An element enclosed in angle brackets (" <" and " >") is a logical syntactic element that shall be defined in another syntax specification. An element enclosed in double angle brackets ("«" and ”»”) shall be a textual description of a single syntactic element. All other non-blank strings except "|" and "{}" (see rules 1 and 5) denote a single syntactic element that shall consist of exactly that character string (or a portion thereof; see rule 6).
 3. The DIBOL grammar is not case sensitive. Any uppercase character that appears in a construct as part of a syntactic element can be replaced by its lowercase representation.
 4. The left side of each syntax definition shall start at the left margin of the document. If the constructs that comprise the right side of the definition are listed on more than one line, the additional lines shall be indented and the definition shall be considered a continuous stream of constructs.
 5. If two syntactic elements are separated by the two "{}" characters, no intervening spaces and/or tabs shall be allowed between the elements. All other syntactic elements can be optionally separated by one or more tabs and/or spaces.
 6. If a syntactic element that is a character string is enclosed by braces ("{" and "}"), then that element may be any substring of the element that begins with the first character.

## 19.2 Source Line Syntax

(1) < logical_line > := < start_line > | < start_line > < continue_lines >
(2) < start_line > := < bol > < eol > | < bol > < not_cchr >{}< line_body > < eol >
(3) < bol > := «beginning of a physical line»
(4) < eol > := «end of a physical line»
(5) < not_cchr > := «any character except an ampersand»
(6) < line_body > := < text > | < text > < comment >
(7) < text > := < text_item > | < text_item > < text >
(8) < text_item > := < quoted_str > | < not_qstr >
(9) < not_qstr > := < nqc_chr > | < nqc_chr > < not_qstr >
(10) < nqc_chr > := «any character except a single quote, double quote, or semicolon»
(11) < comment > := ; | ; < chars >
(12) < chars > := < ascii_char > | < ascii_char > < chars >
(13) < ascii_char > := «any ASCII character»
(14) < continue_lines > := < cont_line > | < cont_line > < continue_lines >
(15) < cont_line > := < bol > & < eol > | < bol > & < line_body > < eol >

## 19.3 Compiler Directive Syntax

(16) < comp_dir > := < define > | < else > | < endc > | < ifdef > | < ifndef > | < include > | < list > | < nolist > | < page > | < title > | < undefine >
(17) < define > := .DEFINE < def_id >
(18) < def_id > := < def_name > | < def_name > , < text >
(19) < def_name > := < ident >
(20) < else > := .ELSE
(21) < endc > := .ENDC
(22) < ifdef > := .IFDEF < ident >
(23) < ifndef > := .IFNDEF < ident >
(24) < include > := .INCLUDE < inc_spec >
(25) < inc_spec > := < caexp > | < caexp > LIBRARY < caexp > | < caexp > < dict_spec >
(26) < dict_spec > := DICTIONARY | DICTIONARY < caexp > | DICTIONARY < caexp > < dict_layout >
(27) < dict_layout > := NORECORD | LITERAL | < dict_struct > | < dict_record > | < dict_common >
(28) < dict_struct > := STRUCTURE | STRUCTURE = < struct_name >
(29) < dict_record > := RECORD | RECORD < dict_nam_ovl >
(30) < dict_nam_ovl > := , X | = < rec_name > | = < rec_name > ,X
(31) < dict_common > := < dict_com_spec > | < dict_com_spec > < dict_nam_ovl >
(32) < dict_com_spec > := COMMON | GLOBAL COMMON | EXTERNAL COMMON
(33) < list > := .LIST
(34) < nolist > := .NOLIST
(35) < page > := .PAGE
(36) < undefine > := .UNDEFINE < def_id >
(37) < title > := .TITLE | .TITLE < caexp >

## 19.4 Routine Syntax

(38) < routine_spec > := < routine > | < routine > < routine_spec >
(39) < routine > := < main_routine > | < sub routine > | < func_routine >
(40) < main_routine > := < main_hdr > < routine_body >
(41) < sub routine > := < sub_hdr > < routine_body >
(42) < func_routine > := < func_hdr > < routine_body >
(43) < main_hdr > := .MAIN < main_spec > < new_line >
(44) < main_spec > := < ident > | < ident > < rt_spec >
(45) < rt_spec > := ROUND | TRUNCATE
(46) < sub_hdr > := .SUBROUTINE < ident > < entry_spec >
(47) < func_hdr > := .FUNCTION < ident > < entry_spec >
(48) < entry_spec > := < entry_type > < arg_spec > | < arg_spec >
(49) < entry_type > := < entry_qual > | < entry_qual > , < entry_type >
(50) < entry_qual > := < rt_spec > | REENTRANT
(51) < arg_spec > := < new_line > | < new_line > < args > < new_line >
(52) < args > := < arg_def > | < arg_def > < new_line > < args >
(53) < arg_def > := < arg > | < dim_arg >
(54) < arg > := < alpha_arg > | < dec_arg > | < impdec_arg > | < int_arg > | < num_arg >
(55) < dim_arg > := < dim_alpha_arg > | < dim_dec_arg > | < dim_impdec_arg > | < dim_int_arg > | < dim_num_arg >
(56) < routine_body > := < data_specs > < routine_code > | < routine_code > 
(57) < data_specs > := < data_def > < new line > | < data_def > < new_line > < data_specs >
(58) < data_def > := < record_def > | < common_def > | < struct_def > | < literal_def > | < ext_func_def >
(59) < routine_code > := .PROC < code_lines > .END < new_line >
(60) < code_lines > := < new_line > | < new_line > < stmnt_line > < code_lines >
(61) < stmnt_line > := < stmnt > | < lbl > , | < lbl > , < stmnt >
(62) < lbl > := < ident >
(63) < stmnt > := < compound_stmnt > | < proc_stmnt >
(64) < compound_stmnt > := BEGIN < code_lines > < end_cmpnd >
(65) < end_cmpnd > := END | < lbl > , END
(66) < proc_stmnt > := < data_stmnt > | < ctl_stmnt > | < io_stmnt > | < comm_stmnt >
(67) < data_stmnt > := < assign > | < clear > | < decr > | < incr > | < init > | < locase > | < map > | < upcase >
(68) < ctl_stmnt > := < call > | < do_until > | < exit > | < exitloop > | < fatal > | < for > | < freturn > | < goto > | < if > | < if_then_else > | < nextloop > | < offerror > | < offtrap > | < onerror > | < repeat > | < return > | < sleep > | < stop > | < trap > | < using > | < while > | < xcall > | < xreturn >
(69) < io_stmnt > := < accept > | < close > | < delete > | < display > | < forms > | < open > | < read > | < reads > | < store > | < unlock > | < write > | < writes >
(70) < comm_stmnt > := < lpque > | < recv > | < send >

## 19.4.1 Routine Arguments

(71) < alpha_arg > := < aarg_name > , A
(72) < aarg_name > := < ident >
(73) < dec_arg > := < darg_name > , < dec_typ >
(74) < dec_typ > := D | O
(75) < darg_name > := < ident >
(76) < impdec_arg > := < idarg_name > , < dec_typ > .
(77) < idarg_name > := < ident >
(78) < int_arg > := < iarg_name > ,I
(79) < iarg_name > := < ident >
(80) < num_arg > := < narg_name > ,N.
(81) < narg_name > := < ident >
(82) < dim_alpha_arg > := < dim_aarg_name > , < arg_dim_spec > A
(83) < arg_dim_spec > := [ < arg_dims > ]
(84) < arg_dims > := * | * , < arg_dims >
(85) < dim_aarg_name > := < ident >
(86) < dim_dec_arg > := < dim_darg_name > , < arg_dim_spec > < dec_typ >
(87) < dim_darg_name > := < ident >
(88) < dim_impdec_arg > := < dim_idarg_name > , < arg_dim_spec > < dec_typ > .
(89) < dim_idarg_name > := < ident >
(90) < dim_int_arg > := < dim_iarg_name > , < arg_dim_spec > I
(91) < dim_iarg_name > := < ident >
(92) < dim_num_arg > := < dim_narg_name > , < arg_dim_spec > N .
(93) < dim_narg_name > := < ident >

## 19.4.2 Records, Commons, and Structures

(94) < record_def > := < record_hdr > < new_line > < field_defs >
(95) < record_hdr > := < record_spec > | < record_spec > ,X
(96) < record_spec > := RECORD | RECORD < rec_name >
(97) < rec_name > := < ident >
(98) < common_def > := < common_hdr > < new_line > < field_defs >
(99) < common_hdr > := < common_spec > | < common_spec > ,X
(100) < common_spec > := < com_type > < com_name >
(101) < com_type > := COMMON | GLOBAL COMMON | EXTERNAL COMMON
(102) < com_name > := < ident >
(103) < struct_def > := STRUCTURE < struct_name > < new_line > < field_defs >
(104) < struct_name > := < ident >
(105) < field_defs > := < field_spec > < new_line > |< field_spec > < new_line> < field_defs >
(106) < field_spec > := < alpha_field > | < dec_field > | < impdec_field > | < int_field > | < unnamed_field > | < group_def >

## 19.4.3 Alpha Fields

(107) < alpha field > := < afld_def > | < dim_afld_def >
(108) < afld_def > := < afld_name > < afld_spec >
(109) < afld_name > := < ident >
(110) < afld_spec > := < fxd_aspec > | < var_aspec >
(111) < fxd_aspec > := , < aspec > | , < aspec > , < alpha_init >
(112) < aspec > := A < cdexp > | < cdexp > A < cdexp >
(113) < alpha_init > := < caexp > | < caexp > , < alpha_init >
(114) < var_aspec > := , A * , < caexp > | , A , < caexp >
(115) < dim_afld_def > := < dim_afld_name > < dim_afld_spec >
(116) < dim_afld_name > := < ident >
(117) < dim_afld_spec > := , < dim_aspec > | , < dim_aspec > , < alpha_init >
(118) < dim_aspec > := < dim_def > A < cdexp >
(119) < dim def > := [ < dim_list > ]
(120) < dim_list > := < cdexp > | < cdexp > , < dim_list >

## 19.4.4 Decimal Fields

(121) < dec_field > := < dfld_def > | < dim_dfld_def >
(122) < dfld_def > := < dfld_name > < dfld_spec >
(123) < dfld_name > := < ident >
(124) < dfld_spec > := < fxd_dspec > | < var_dspec >
(125) < fxd_dspec > := , < dspec > | , < dspec >, < dec_init >
(126) < dspec > := < dec_typ > < cdexp > |< cdexp > < dec_typ > < cdexp >
(127) < dec_init > := < cnexp > | < cnexp > ,< dec_init >
(128) < txt_nlit > := < txt_dlit > | < txt_idlit >
(129) < sign > := + | -
(130) < var_dspec > := , < dec_typ > *, < cnexp > | , < dec_typ > , < cdexp >
(131) < dim_dfld_def > := < dim_dfld_name > < dim_dfld_spec >
(132) < dim_dfld_name > := < ident >
(133) < dim_dfld_spec > := , < dim_dspec > | , < dim_dspec > , < dec_init >
(134) < dim_dspec > := < dim_def > < dec_typ > < cdexp >

## 19.4.5 Implied Decimal Fields

(135) < impdec_field > := < idfld_def > | < dim_idfld_def >
(136) < idfld_def > := < idfld_name > < idfld_spec >
(137) < idfld_name > := < ident >
(138) < idfld_spec > := < fxd_idspec > | < var_idspec >
(139) < fxd_idspec > := , < idspec > | , < idspec > , < id_init >
(140) < idspec > := < dec_typ > < idsiz > | < cdexp > < dec_typ > < idsiz >
(141) < idsiz > := < cdexp >{}.{}< cdexp >
(142) < id_init > := < cnexp > | < cnexp > , < id_init >
(143) < var_idspec > := , < dec_typ > *{}.{}*, < cnexp > | , < dec_typ > , < cidexp >
(144) < dim_idfld_def > := < dim_idfld_name > < dim_idfld_spec >
(145) < dim_idfld_name > := < ident >
(146) < dim_idfld_spec > := , < dim_idspec > | , < dim_idspec > , < id_init >
(147) < dim_idspec > := < dim_def > < dec_typ > < idsiz >

## 19.4.6 Integer Fields

(148) < int_field > := < ifld_def > | < dim_ifld_def >
(149) < ifld_def > := < ifld_name > < ifld_spec >
(150) < ifld_name > := < ident >
(151) < ifld_spec > := < fxd_ispec > | < var_ispec >
(152) < fxd_ispec > := , < ispec > | , < ispec > , < int_init >
(153) < ispec > := I < cdexp > | < cdexp > I < cdexp >
(154) < int_init > := < cnexp > | < cnexp > , < int_init >
(155) < var_ispec > := ,I*, < cnexp > | ,I, < cnexp >
(156) < dim_ifld_def > := < dim_ifld_name > < dim_ifld_spec >
(157) < dim_ifld_name > := < ident >
(158) < dim_ifld_spec > := , < dim_ispec > | , < dim_ispec > , < dec_init >
(159) < dim_ispec > := < dim_def > I < cdexp >

## 19.4.7 Unnamed Fields

(160) < unnamed_field > := < afld_spec > | < dim_afld_spec > | < dfld_spec > | < dim_dfld_spec > | < idfld_spec > | < dim_idfld_spec > | < ifld_spec > | < dim_ifld_spec >

## 19.4.8 Groups

(161) < group_def > := GROUP < group_body > ENDGROUP
(162) < group_body > := < group_type > < new_line > < field_defs > | < new_line > < field_defs >
(163) < group_type > := < group_spec > | < group_spec > , X | , X
(164) < group_spec > := < agroup_spec > | < agroup_spec > | < idgroup_spec > | < igroup_spec >
(165) < agroup_spec > := < agrp_name > | < agrp_name > , < agrp_typ > | < dim_agrp_name > , < dim_def > < agrp_typ >
(166) < agrp_typ > := A | A < cdexp >
(167) < agrp_name > := < ident >
(168) < dim_agrp_name > := < ident >
(169) < dgroup_spec > := < dgrp_name > , < dgrp_typ > | < dim_dgrp_name > , < dim_def > < dgrp_typ >
(170) < dgrp_typ > := < dec_typ > | < dec_typ > < cdexp >
(171) < dgrp_name > := < ident >
(172) < dim_dgrp_name > := < ident >
(173) < idgroup_spec > := < idgrp_name > , < idgrp_typ > | < dim_idgrp_name > , < dim_def > < idgrp_typ >
(174) < idgrp_typ > := < dec_typ > | < dec_typ > < idsiz >
(175) < idgrp_name > := < ident >
(176) < dim_idgrp_name > := < ident >
(177) < igroup_spec > := < igrp_name > , < igrp_typ > | < dim_igrp_name > , < dim_def > < igrp_typ >
(178) < igrp_typ > := I | I < cdexp >
(179) < igrp_name > := < ident >
(180) < dim_igrp_name > := < ident >

## 19.4.9 Data Literals

(181) < literal_def > := < literal_hdr > < new_line > < lit_defs >
(182) < literal_hdr > := < literal_spec > | < literal_spec > , X
(183) < literal_spec > := LITERAL | LITERAL < lit_name >
(184) < lit_name > := < ident >
(185) < lit_defs > := < lfld_spec > < new_line > | < lfld_spec > < new_line > < lit_defs >
(186) < lfld_spec > := < lit_afld > | < lit_dfld > | < lit_idfld > | < lit_ifld > | < unnamed_field > | < lit_group >
(187) < lit_afld > := < lit_afld_def > | < ldim_afld_def >
(188) < lit_afld_def > := < lit_afld_name > < afld_spec >
(189) < lit_afld_name > := < ident >
(190) < ldim_afld_def > := < ldim_afld_name > < dim_afld_spec >
(191) < ldim_afld_name > := < ident >
(192) < lit_dfld > := < lit_dfld_def > | < ldim_dfld_def >
(193) < lit_dfld_def > := < lit_dfld_name > < dfld_spec >
(194) < lit_dfld_name > := < ident >
(195) < ldim_dfld_def > := < ldim_dfld_name > < dim_dfld_spec >
(196) < ldim_dfld_name > := < ident >
(197) < lit_idfld > := < lit_idfld_def > | < ldim_idfld_def >
(198) < lit_idfld_def > := < lit_idfld_name > < idfld_spec >
(199) < lit_idfld_name > := < ident >
(200) < ldim_idfld_def > := < ldim_idfld_nam > < dim_idfld_spec >
(201) < ldim_idfld_nam > := < ident >
(202) < lit_ifld > := < lit_ifld_def > | < ldim_ifld_def >
(203) < lit_ifld_def > := < lit_ifld_name > < ifld_spec >
(204) < lit_ifld_name > := < ident >
(205) < ldim_ifld_def > := < ldim_ifld_name > < dim_ifld_spec >
(206) < ldim_ifld_name > := < ident >
(207) < lit_group > := GROUP < lit_grp_body > ENDGROUP
(208) < lit_grp_body > := < lit_grp_type > < new_line > < lit_defs > | < new_line > < lit_defs >
(209) < lit_grp_type > := < lit_grp_spec > | < lit_grp_spec > , X | ,X
(210) < lit_grp_spec > := < lit_agrp_spec > | < lit_dgrp_spec > | < lit_idgrp_spec > | < lit_igrp_spec >
(211) < lit_agrp_spec > := < lit_agrp_name > | < lit_agrp_name > , < agrp_typ > | < ldim_agrp_name > , < dim_def > < agrp_typ >
(212) < lit_agrp_name > := < ident >
(213) < ldim_agrp_name > := < ident >
(214) < lit_dgrp_spec > := < lit_dgrp_name > , < dgrp_typ > | < ldim_dgrp_name > , < dim_def > < dgrp_typ >
(215) < lit_dgrp_name > := < ident >
(216) < ldim_dgrp_name > := < ident >
(217) < lit_idgrp_spec > := < lit_idgrp_name > , < idgrp_typ > | < ldim_idgrp_nam > , < dim_def > < idgrp_typ >
(218) < lit_idgrp_name > := < ident >
(219) < ldim_idgrp_nam > := < ident >
(220) < lit_igrp_spec > := < lit_igrp_name > , < igrp_typ > | < ldim_igrp_name > , < dim_def > < igrp_typ >
(221) < lit_igrp_name > := < ident >
(222) < ldim_igrp_name > := < ident >

## 19.4.10 External Functions

(223) < ext_func_def > := EXTERNAL FUNCTION < new_line > < fiinc_specs >
(224) < func_specs > := < func_id > < new_line > | < func_id > < new_line > < func_specs >
(225) < func_id > := < alpha_func > | < dec_func > | < impdec_func > | < int_func >
(226) < alpha_func > := < afnc_name > ,A
(227) < afnc_name > := < ident >
(228) < dec_func > := < dfnc_name > , < dec_typ >
(229) < dfnc_name > := < ident >
(230) < impdec_func > := < idfnc_name > , < dec_typ > .
(231) < idfnc_name > := < ident >
(232) < int_func > := < ifnc_name > ,1
(233) < ifnc_name > := < ident >

## 19.4.11 Data Change Statements
(234) < assign > := < var > = | < var > = < exp > | < avar > = < nexp > , < aexp >
(235) < clear > := CLEAR < var_list >
(236) < var_list > := < var > | < var > , < var_list >
(237) < decr > := DECR < nvar >
(238) < incr > := INCR < nvar >
(239) < init > := INIT < var_list >
(240) < locase > := LOCASE < avar >
(241) < map > := MAP ( < struct_name > , < var > )
(242) < upcase > := UPCASE < avar >

## 19.4.12 Program Control Statements

(243) < call > := CALL < lbl >
(244) < do_until > := DO < stmnt_part > < until_part >
(245) < stmnt_part > := < stmnt > | < new_line > < stmnt > | < new_line > < lbl > , < stmnt >
(246) < until_part > := UNTIL < exp > | < new_line > UNTIL < exp >
(247) < exit > := EXIT
(248) < exitloop > := EXITLOOP
(249) < fatal > := FATAL | FATAL < ident >
(250) < for > := FOR < nvar > < range_part > < stmnt_part >
(251) < range_part > := < start_part > | < start_part > BY < nexp >
(252) < start_part > := FROM < nexp > THRU < nexp >
(253) < freturn > := FRETURN < exp >
(254) < goto > := GOTO < lbl > | GOTO ( < label_list > ), < nexp >
(255) < label_list > := < lbl > | < lbl > , < label_list >
(256) < if > := IF < exp > < stmnt_part >
(257) < if_then_else > := IF < exp > < then_part > < else_part >
(258) < then_part > := THEN < stmnt_part > | < new_line > THEN < stmnt_part >
(259) < else_part > := ELSE < stmnt_part > | < new_line > ELSE < stmnt_part >
(260) < nextloop > := NEXTLOOP
(261) < offerror > := OFFERROR
(262) < offtrap > := OFFTRAP
(263) < onerror > := ONERROR < lbl >
(264) < repeat > := REPEAT < stmnt_part >
(265) < return > := RETURN
(266) < sleep > := SLEEP < nexp >
(267) < stop > := STOP | STOP < aexp >
(268) < trap > := TRAP < stmnt_part > INTO < stmnt_part >
(269) < using > := USING < exp > SELECT < using_body > ENDUSING
(270) < using_body > := < new_line > < using_list >
(271) < using_list > := < using_entry > | < using_entry > < using_list >
(272) < using_entry > := < match_spec > , < stmnt_part > < new_line >
(273) < match_spec > := ( < match_list > ) | ()
(274) < match_list > := < match_entry > | < match_entry > , < match_list >
(275) < match_entry > := < exp > | < exp > THRU < exp > | < r_op > < exp > | < s_op > < aexp >
(276) < while > := WHILE < exp > < stmnt_part >
(277) < xcall > := XCALL < ident > | XCALL < ident > ( < xarg_list > )
(278) < xarg_list > := < xcall_arg > | < xcall_arg > , < xarg_list >
(279) < xcall_arg > := < exp > | ^DESCR ( < exp > ) | ^REF ( < exp > ) | ^VAL ( < exp > ) | < dim_var > < arg_dim_spec >
(280) < dim_var > := < dim_narg_name > | < dim_anam_ref > | < dim_dnam_ref > | < dim_idnam_ref > | < dim_inam_ref >
(281) < xreturn > := XRETURN

## 19.4.13 I/O Statements

(282) < accept > := ACCEPT ( < nexp >, < acpt_spec > )
(283) < acpt_spec > := < var > | < var > , < lbl >
(284) < close > := CLOSE < nexp >
(285) < delete > := DELETE ( < nexp > )
(286) < display > := DISPLAY ( < nexp > , < exp_list > )
(287) < exp_list > := < exp > | < exp > , < exp_list >
(288) < forms > := FORMS ( < nexp > , < nexp > )
(289) < open > := OPEN ( < nexp > , < open_mode > , < open_file > )
(290) < open_mode > := < mode > : < sub_mode > | {MODE} : < aexp >
(291) < mode > := {INPUT} | {OUTPUT} | {UPDATE}
(292) < sub_mode > := (SEQUENTIAL) | (RELATIVE) | {INDEXED} | {PRINT} | {CHARACTER}
(293) < open_file > := < aexp > | < aexp > , < open_options >
(294) < open_options > := < opn_opt > | < opn_opt > , < open_options >
(295) < opn_opt > := ALLOC : < nexp > | BKTSIZ : < nexp > | BLKSIZ : < nexp > | BUFSIZ : < nexp > | RECSIZ : < nexp > | NUMREC : < nexp >
(296) < read > := READ ( < nexp > , < avar > , < read_qual > )
(297) < read_qual > := < exp > | < aexp > | < aexp > , KEYNUM : < nexp >
(298) < reads > := READS ( < nexp >, < reads_spec > )
(299) < reads_spec > := < avar > | < avar > , < lbl >
(300) < store > := STORE ( < nexp > , < aexp > )
(301) < unlock > := UNLOCK < nexp >
(302) < write > := WRITE ( < nexp > , < write_spec > )
(303) < write_spec > := < aexp > | < aexp > , < nexp >
(304) < writes > := WRITES ( < nexp > , < aexp > )

## 19.4.14 System Communication Statements

(305) < lpque > := LPQUE ( < lpque_req > )
(306) < lpque_req > := < aexp > | < aexp > , < lpque_optlist >
(307) < lpque_optlist > := < lpque_opt> | < lpque_opt> , < lpque_optlist>
(308) < lpque_opt > := LPNUM : <exp> | COPIES : < nexp> | FORM : < exp> | DELETE | DELETE : < nexp>
(309) < recv > := RECV ( < avar > , < recv_spec > )
(310) < recv spec > := < lbl > | < lbl > , < nvar > | < lbl > , < aexp > , < nvar >
(311) < send > := SEND ( < aexp > , < dest_spec > )
(312) < dest_spec > := < aexp > | < aexp > , < aexp >

## 19.4.15 Data Reference

(313) < var > := < avar > | < nvar >
(314) < avar > := < avar_ref > | < avar_ref > < sub_field > |^A(< var >)
(315) < avar_ref > := < aarg_spec > | < rcs_name > | < afld_ref > | < path >{}.{}< afld_ref >
(316) < aarg_spec > := < aarg_name > | ^ARG ( < nexp > )
(317) < rcs_name > := < rec_name > | < com_name > | < struct_name >
(318) < afld_ref > := < anam_ref > | < dim_anam_ref > < dim_ref > | < dim_anam_ref > []
(319) < anam_ref > := < afld_name > | < agrp_name >
(320) < dim_anam_ref > := < dim_afld_name > | < dim_agrp_name > | < dim_aarg_name >
(321) < nvar > := < narg > | < dvar > | < idvar > | < ivar >
(322) < narg > := < narg_ref > | < narg_ref > < sub_field >
(323) < narg ref > := < narg_name > | < dim_narg_name > < dim_ref > | < dim_narg_name > ~ [ ] | ^ARGN ( < nexp > )
(324) < dvar > := < dvar_ref > | < dvar_ref > < sub_field > | ^{}< dec_typ > ( < var > )
(325) < dvar_ref > := < darg_name > | < dim_darg_name > < dim_ref > | < dim_darg_name > [] | < dfld_ref > | < path >{}.{}< dfld_ref >
(326) < dfld_ref > := < dnam_ref > | < dim_dnam_ref > < dim_ref > | < dim_dnam_ref > [ ]
(327) < dnam_ref > := < dfld_name > | < dgrp_name >
(328) < dim_dnam_ref > := < dim_dfld_name > | < dim_dgrp_name >
(329) < idvar > := < idvar_ref > | < idvar_ref > < sub_field > | ^{}< dec_typ >( < var > , < nexp > )
(330) < idvar_ref > := < idarg_name > | < dim_idarg_name > < dim_ref > | < dim_idarg_name > [] | < idfld_ref > | < path >{}.{}< idfld_ref >
(331) < idfld_ref > := < idnam_ref > | < dim_idnam_ref > < dim_ref > | < dim_idnam_ref > [ ]
(332) < idnam_ref > := < idfld_name > | < idgrp_name >
(333) < dim_idnam_ref > := < dim_idfld_name > | < dim_idgrp_name >
(334) < ivar > := < ivar_ref > | < ivar_ref > < sub_field > |^I(< var >)
(335) < ivar_ref > := < iarg_name > | < dim_iarg_name > < dim_ref > | < dim_iarg_name > [] | < ifld_ref > | < path > {}.{} < ifld_ref >
(336) < ifld_ref > := < inam_ref > | < dim_inam_ref > < dim_ref > | < dim_inam_ref > [ ]
(337) < inam_ref > := < ifld_name > | < igrp_name >
(338) < dim_inam_ref > := < dim_ifld_name > | < dim_igrp_name >

## 19.4.16 Paths, Dimensions, and Sub-fields

(339) < path > := < rcs_name > | < group_path > | < rcs_name >{}.{}< group_path >
(340) < group_path > := < group_ref > | < group_ref > {}.{}< group_path >
(341) < group_ref > := < group_name > | < dim_grp_name > < dim_ref >
(342) < group_name > := < agrp_name > | < dgrp_name > | < idgrp_name > | < igrp_name >
(343) < dim_grp_name > := < dim_agrp_name > | < dim_dgrp_name > | < dim_idgrp_name > | < dim_igrp_name >
(344) < dim_ref > := [ < dims > ]
(345) < dims > := < nexp > | < nexp > , < dims >
(346) < sub_field > := ( < nexp > ) | ( < nexp > , < nexp > ) | ( < nexp > : < nexp > )

## 19.4.17 Expressions

(347) < exp > := < aexp > | < nexp >
(348) < aexp > := < aref > | < aref > < plsmns > < aexp > | (< aexp >)
(349) < plsmns > := + | -
(350) < aref > := < avar > | < afhc_ref > | < alit >
(351) < afhc_ref > := %{} < afhc_name > | %{} < afhc_name > ( < xarg_list > ) | ^ A ( < fhc_ref > )
(352) < fhc_ref > := < afhc_ref > | < nfhc_ref >
(353) < nexp > := < bexp > | < lexp > | < bitexp > | < decexp > | ( < nexp > )
(354) < bexp > := < exp > < b_op > < exp > | .NOT. < exp >
(355) < b_op > := .AND. | .OR. | .XOR.
(356) < lexp > := < aexp > < s_op > < aexp > | < nexp > < r_op > < nexp >
(357) < s_op > := < r_op > | .EQS. | .NES. | .LTS. | .LES. | .GTS. | .GES.
(358) < r_op > := .EQ. | .NE. | .LT. | .LE. | .GT. | .GE.
(359) < bitexp > := < iref > | < iref > < i_op > < bitexp > | .BNOT. < bitexp >
(360) < i_op > := .BAND. | .BOR. | .BNAND. | .BXOR.
(361) < iref > := < ivar > | < ifhc_ref > | < dlit >
(362) < decexp > := < nref > | < nref > < d_op > < nexp > | < plsmns > < nexp >
(363) < d_op > := \ I \ * \ #
(364) < nref > := < nvar > | < nfhc_ref > | < nlit_ref > | < data_param >
(365) < nfhc_ref > := < dfhc_ref > | < idfhc_ref > | < ifhc_ref >
(366) < dfhc_ref > := %{}< dfhc_name > | %{}< dfhc_name > ( < xarg_list > ) | ^{} < dec_typ > ( < fhc_ref > )
(367) < idfhc_ref > := %{} < idfhc_name > | %{} < idfhc_name > ( < xarg_list > ) | ^{}< dec_typ > ( < fhc_ref > , < nexp > )
(368) < ifhc_ref > := %{} < ifhc_name > | %{}< ifhc_name > ( < xarg_list > ) | ^I ( < fhc_ref > )
(369) < data_param > := ^ADDR( < var > ) | ^MAPPED( < struct_id > ) | ^PASSED( < arg_name > ) | ^SIZE( < exp > )
(370) < struct_id > := < struct_name > | < struct_member >
(371) < struct_member > := «a < var > defined by a < field_spec > within a STRUCTURE»
(372) < arg_name > := < aarg_name > | < dim_aarg_name > | < darg_name > | < dim_darg_name > | < iarg_name > | < dim_iarg_name > | < idarg_name > | < dim_idarg_name > | < narg_name > | < dim_narg_name >
## 19.4.18 Literals

(373) < alit > := < data_alit > | < quoted_str >
(374) < data_alit > := < lit_avar_ref > | < lit_avar_ref > < sub_field >
(375) < lit_avar_ref > := < lit_name > | < lit_afld_ref > | < lpath >{}.{}< lit_afld_ref >
(376) < lit_afld_ref > := < lit_anam_ref > | < ldim_anam_ref > < dim_ref > | < ldim_anam_ref > [ ]
(377) < lit_anam_ref > := < lit_afld_name > | < lit_agrp_name >
(378) < ldim_anam_ref > := < ldim_afld_name > | < ldim_agrp_name >
(379) < lpath > := < lit_name > | < lit_grp_path > | < lit_name >{}.{}< lit_grp_path >
(380) < lit_grp_path > := < lit_grp_ref > | < lit_grp_ref >{}.{}< lit_grp_path >
(381) < lit_grp_ref > := < lit_grp_name > | < ldim_grp_name > < dim_ref >
(382) < lit_grp_name > := < lit_agrp_name > | < lit_dgrp_name > | < lit_idgrp_name > | < lit_igrp_name >
(383) < ldim_grp_name > := < ldim_agrp_name > | < ldim_dgrp_name > | < ldim_idgrp_nam > | < ldim_igrp_name >
(384) < quoted_str > := < dq_string > | < sq_string >
(385) < dq_string > := "{}< dq_chars >{}" | "{}"
(386) < dq_chars > := < dq_chr > | < dq_chr > < dq_chars >
(387) < dq_chr > := "{}" | < not_dq >
(388) < not_dq > := «any ASCII character except a double quote»
(389) < sq_string > := ’{}< sq_chars >{}’ | ’{}’
(390) < sq_chars > := < sq_chr > | < sq_chr > < sq_chars >
(391) < sq_chr > := ’{}’ | < not_sq >
(392) < not_sq > := «any ASCII character except a single quote»
(393) < nlit_ref > := < dlit > | < idlit > | < ilit >
(394) < dlit > := < data_dlit > | < txt_dlit > | < err_dlit >
(395) < data_dlit > := < lit_dvar_ref > | < lit_dvar_ref > < sub_field >
(396) < lit_dvar_ref > := < lit_dfld_ref > | < lpath >{}.{}< lit_dfld_ref >
(397) < lit_dfld_ref > := < lit_dnam_ref > | < Idim_dnam_ref > < dim_ref > | < Idim_dnam_ref > []
(398) < lit_dnam_ref > := < lit_dfld_name > | < lit_dgrp_name >
(399) < Idim_dnam_ref > := < ldim_dfld_name > | < ldim_dgrp_name >
(400) < txt_dlit > := < dchars >
(401) < dchars > := < digit_char > | < digit_char >{}< dchars >
(402) < digit_char > := 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
(403) < err_dlit > := $ERR_{}< err_ident >
(404) < err_ident > := «DIBOL error literal identifier (see section 18)»
(405) < idlit > := < data_idlit > | < txt_idlit >
(406) < data_idlit > := < lit_idvar_ref > | < lit_idvar_ref > < sub_field >
(407) < lit_idvar_ref > := < lit_idfld_ref > | < lpath >{}.{}< lit_idfld_ref >
(408) < lit_idfld_ref > := < lit_idnam_ref > | < ldim_idnam_ref > < dim_ref > | < ldim_idnam_ref > [ ]
(409) < lit_idnam_ref > := < lit_idfld_name > | < lit_idgrp_name >
(410) < ldim_idnam_ref > := < ldim_idfld_nam > | < ldim_idgrp_nam >
(411) < txt_idlit > := < dchars >{}.{}< dchars >
(412) < ilit > := < lit_ivar_ref > | < lit_ivar_ref > < sub_field >
(413) < lit_ivar_ref > := < lit_ifld_ref > | < lpath > {}.{} < lit_ifld_ref >
(414) < lit_ifld_ref > := < lit_inam_ref > | < ldim_inam_ref > < dim_ref > | < ldim_inam_ref > [ ]
(415) < lit_inam_ref > := < lit_ifld_name > | < lit_igrp_name >
(416) < ldim_inam_ref > := < ldim_ifld_name > | < ldim_igrp_name >

## 19.4.19 Miscellaneous Definitions

(417) < ident > := < alpha_char > | < alpha_char >{}< ident_chars >
(418) < alpha_char > := A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z
(419) < ident_chars > := < ident_chr > | < ident_chr >{}< ident_chars >
(420) < ident_chr > := < alpha_char > | < digit_char > | $ | _
(421) < new_line > := «end of a logical line»
(422) < caexp > := «an < aexp > that can be completely evaluated at compilation time»
(423) < cnexp > := «an < nexp > that can be completely evaulated at compilation time»
(424) < cdexp > := «a < cnexp > whose type is decimal»»
(425) < cidexp > := «a < cnexp > whose type is implied decimal»
