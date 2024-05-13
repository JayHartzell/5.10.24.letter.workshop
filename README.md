# 5.10.24.letter.workshop
LSP Systems Work Group Letter Configuration Workshop May 10, 2024

#### Loan Status Notice
Problem: Conditionally send unique text to patrons when the due date is changed on a laptop or hotspot.
Solution: Under the `DUE_DATE_CHANGE_ONLY` condition, add a separate test for material type. When the material type matches, render the placeholder text.

XSL:
`<xsl:if test="notification_data/message='DUE_DATE_CHANGE_ONLY'">
                                <xsl:choose>
                                    <xsl:when test="/notification_data/item_loans/item_loan/physical_item/material_type = 'LPTOP' or /notification_data/item_loans/item_loan/physical_item/material_type = 'MOBILEDEVICE'">
                                        Library staff has extended the loan period for the equipment listed below. No action is needed on your part.
                                    </xsl:when>
                                    <xsl:otherwise>
                                        <strong>@@message@@</strong>
                                    </xsl:otherwise>
                                </xsl:choose>
                            </xsl:if>`


Problem: For the same material types, change the library displayed in the footer to static text for a laptop loan program.
Solution: Wrap the footer in a conditional statement, and add a template to the `footer.xsl` component that is rendered when the notice is for a laptop or hotspot.

XSL:
`<xsl:choose>
<xsl:when test="/notification_data/item_loans/item_loan/physical_item/material_type = 'LPTOP' or /notification_data/item_loans/item_loan/physical_item/material_type = 'MOBILEDEVICE'">
<xsl:call-template name="techFooter" /> <!-- techfooter.xsl -->
</xsl:when>
<xsl:otherwise>
<xsl:call-template name="lastFooter" />
</xsl:otherwise>
</xsl:choose>`

#### footer.xsl template
`<xsl:template name="techFooter">
<xsl:call-template name="myAccount" />
 <xsl:call-template name="contactUs" />`

	<table>
	<xsl:attribute name="style">
		<xsl:call-template name="footerTableStyleCss" /> <!-- style.xsl -->
	</xsl:attribute>
	<tr>
	<xsl:for-each select="notification_data/organization_unit">

		<xsl:attribute name="style">
			<xsl:call-template name="listStyleCss" /> <!-- style.xsl -->
		</xsl:attribute>
			<td align="center">Laptop Loan Program&#160;<xsl:value-of select="line1"/>&#160;<xsl:value-of select="line2"/>&#160;<xsl:value-of select="city"/>&#160;<xsl:value-of select="postal_code"/>&#160;<xsl:value-of select="country"/></td>

	</xsl:for-each>
	</tr>
	</table>
`</xsl:template>`
